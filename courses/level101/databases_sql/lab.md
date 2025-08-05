**先決條件**

安裝 Docker

**設定**

建立一個工作目錄，命名為 `sos` 或其他相似名稱，然後進入該目錄 (`cd`)。

在 `custom` 資料夾下建立一個名為 `my.cnf` 的檔案，內容如下：

```shell
sos $ cat custom/my.cnf
[mysqld]

# 這些設定適用於 MySQL 伺服器
# 可以設定連接埠、socket 路徑、緩衝區大小等
# 以下我們設定慢查詢相關參數

slow_query_log=1
slow_query_log_file=/var/log/mysqlslow.log
long_query_time=1
```

啟動容器並啟用慢查詢日誌：

```shell
sos $ docker run --name db -v custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=realsecret -d mysql:8
sos $ docker cp custom/my.cnf $(docker ps -qf "name=db"):/etc/mysql/conf.d/custom.cnf
sos $ docker restart $(docker ps -qf "name=db")
```

匯入示範資料庫：

```shell
sos $ git clone git@github.com:datacharmer/test_db.git
sos $ docker cp test_db $(docker ps -qf "name=db"):/home/test_db/
sos $ docker exec -it $(docker ps -qf "name=db") bash
root@3ab5b18b0c7d:/# cd /home/test_db/
root@3ab5b18b0c7d:/# mysql -uroot -prealsecret mysql < employees.sql
root@3ab5b18b0c7d:/etc# touch /var/log/mysqlslow.log
root@3ab5b18b0c7d:/etc# chown mysql:mysql /var/log/mysqlslow.log
```

_工作坊 1：執行一些範例查詢_

請執行以下指令：

```shell
$ mysql -uroot -prealsecret mysql
mysql>

# 查看資料庫與資料表
# 最後 4 個是 MySQL 系統內部資料庫

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| employees          |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+

mysql> USE employees;
mysql> SHOW TABLES;
+----------------------+
| Tables_in_employees  |
+----------------------+
| current_dept_emp     |
| departments          |
| dept_emp             |
| dept_emp_latest_date |
| dept_manager         |
| employees            |
| salaries             |
| titles               |
+----------------------+

# 讀取幾筆資料
mysql> SELECT * FROM employees LIMIT 5;

# 依條件篩選資料
mysql> SELECT COUNT(*) FROM employees WHERE gender = 'M' LIMIT 5;

# 計算特定資料數量
mysql> SELECT COUNT(*) FROM employees WHERE first_name = 'Sachin'; 
```

_工作坊 2：使用 explain 和 explain analyze 分析查詢，識別並加入索引以提升效能_

```shell
# 查看資料表上的所有索引
# (\G 會水平輸出，改用 ; 可取得表格格式輸出)

mysql> SHOW INDEX FROM employees FROM employees\G
*************************** 1. row ***************************
        Table: employees
   Non_unique: 0
     Key_name: PRIMARY
 Seq_in_index: 1
  Column_name: emp_no
    Collation: A
  Cardinality: 299113
     Sub_part: NULL
       Packed: NULL
         Null:
   Index_type: BTREE
      Comment:
Index_comment:
      Visible: YES
   Expression: NULL

# 這個查詢使用索引，由 'key' 欄位表示
# 加上 explain 關鍵字前置後，
# 可看到查詢計畫（包含所用索引）

mysql> EXPLAIN SELECT * FROM employees WHERE emp_no < 10005\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: employees
   partitions: NULL
         type: range
possible_keys: PRIMARY
          key: PRIMARY
      key_len: 4
          ref: NULL
         rows: 4
     filtered: 100.00
        Extra: Using where

# 與下方不使用索引的查詢做比較

mysql> EXPLAIN SELECT first_name, last_name FROM employees WHERE first_name = 'Sachin'\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: employees
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 299113
     filtered: 10.00
        Extra: Using where

# 查看該查詢耗時

mysql> EXPLAIN ANALYZE SELECT first_name, last_name FROM employees WHERE first_name = 'Sachin'\G
*************************** 1. row ***************************
EXPLAIN: -> Filter: (employees.first_name = 'Sachin')  (cost=30143.55 rows=29911) (actual time=28.284..3952.428 rows=232 loops=1)
    -> Table scan on employees  (cost=30143.55 rows=299113) (actual time=0.095..1996.092 rows=300024 loops=1)


# 計畫成本 (query planner 預估) 為 30143.55
# 實際時間為首筆 28.284 毫秒，全部 3952.428 毫秒
# 現在新增索引並重新執行查詢

mysql> CREATE INDEX idx_firstname ON employees(first_name);
Query OK, 0 rows affected (1.25 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> EXPLAIN ANALYZE SELECT first_name, last_name FROM employees WHERE first_name = 'Sachin';
+--------------------------------------------------------------------------------------------------------------------------------------------+
| EXPLAIN                                                                                                                                    |
+--------------------------------------------------------------------------------------------------------------------------------------------+
| -> Index lookup on employees using idx_firstname (first_name='Sachin')  (cost=81.20 rows=232) (actual time=0.551..2.934 rows=232 loops=1)
 |
+--------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.01 sec)

# 實際時間為第一筆 0.551 毫秒，全筆 2.934 毫秒，效能大幅提升！
# 同時可見該查詢僅使用索引搜尋，無需掃描整張資料表，
# 大幅減少對資料庫的負擔。
```

_工作坊 3：在 MySQL 伺服器上找出慢查詢_

```shell
# 在兩個終端機視窗各執行下面的指令，開啟兩個 shell 進入容器

$ docker exec -it $(docker ps -qf "name=db") bash

# 在其中一個 shell 中開啟 `mysql` 互動介面並執行指令
# 我們設定只紀錄耗時超過 1 秒的查詢，
# 因此這個 `sleep(3)` 會被記錄下來

$ mysql -uroot -prealsecret mysql
mysql> select sleep(3);

# 接著在另一個終端機中，使用 tail 指令查看慢查日誌細節

root@62c92c89234d:/etc# tail -f /var/log/mysqlslow.log
/usr/sbin/mysqld, Version: 8.0.21 (MySQL Community Server - GPL). started with:
Tcp port: 3306  Unix socket: /var/run/mysqld/mysqld.sock
Time                 Id Command    Argument

# Time: 2020-11-26T14:53:44.822348Z
# User@Host: root[root] @ localhost []  Id:     9
# Query_time: 5.404938  Lock_time: 0.000000 Rows_sent: 1  Rows_examined: 1
use employees;
# Time: 2020-11-26T14:53:58.015736Z
# User@Host: root[root] @ localhost []  Id:     9
# Query_time: 10.000225  Lock_time: 0.000000 Rows_sent: 1  Rows_examined: 1

SET timestamp=1606402428;
select sleep(3);
```

以上為模擬且簡化的範例，實務中查詢通常會更複雜，利用 explain/analyze 與慢查詢日誌可帶來更豐富的細節與分析依據。

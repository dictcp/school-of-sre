### SELECT 查詢
在使用 MySQL 時最常用的指令是 `SELECT`。它用來從一個或多個資料表中擷取結果集。  
典型的 Select 查詢的一般格式看起來像這樣：

```
SELECT expr
FROM table1
[WHERE condition]
[GROUP BY column_list HAVING condition]
[ORDER BY column_list ASC|DESC]
[LIMIT #]
```

上述一般格式包含了一些 `SELECT` 查詢中常用的子句：

- **expr** - 以逗號分隔的欄位清單，或使用 *（表示所有欄位）
- **WHERE** - 提供條件，若為真，則指示查詢只選取符合條件的紀錄。
- **GROUP BY** - 根據指定的欄位列表將整個結果集分組。建議在查詢的選擇表達式中使用聚合函數。**HAVING** 用於分組後，對選定欄位或任何其它聚合函數施加條件。
- **ORDER BY** - 根據欄位列表進行結果排序，升冪或降冪。
- **LIMIT** - 常用於限制回傳的紀錄數量。

以下讓我們透過一些範例更好地理解上述內容。以下範例所使用的資料集可在[這裡](https://dev.mysql.com/doc/employee/en/employees-installation.html)免費下載使用。

**選取全部紀錄**

```shell
mysql> SELECT * FROM employees LIMIT 5;
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
|  10001 | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |
|  10002 | 1964-06-02 | Bezalel    | Simmel    | F      | 1985-11-21 |
|  10003 | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28 |
|  10004 | 1954-05-01 | Chirstian  | Koblick   | M      | 1986-12-01 |
|  10005 | 1955-01-21 | Kyoichi    | Maliniak  | M      | 1989-09-12 |
+--------+------------+------------+-----------+--------+------------+
5 rows in set (0.00 sec)
```

**選取所有紀錄的特定欄位**

```shell
mysql> SELECT first_name, last_name, gender FROM employees LIMIT 5;
+------------+-----------+--------+
| first_name | last_name | gender |
+------------+-----------+--------+
| Georgi     | Facello   | M      |
| Bezalel    | Simmel    | F      |
| Parto      | Bamford   | M      |
| Chirstian  | Koblick   | M      |
| Kyoichi    | Maliniak  | M      |
+------------+-----------+--------+
5 rows in set (0.00 sec)
```

**選取 hire_date >= 1990年1月1日 的所有紀錄**

```shell
mysql> SELECT * FROM employees WHERE hire_date >= '1990-01-01' LIMIT 5;
+--------+------------+------------+-------------+--------+------------+
| emp_no | birth_date | first_name | last_name   | gender | hire_date  |
+--------+------------+------------+-------------+--------+------------+
|  10008 | 1958-02-19 | Saniya     | Kalloufi    | M      | 1994-09-15 |
|  10011 | 1953-11-07 | Mary       | Sluis       | F      | 1990-01-22 |
|  10012 | 1960-10-04 | Patricio   | Bridgland   | M      | 1992-12-18 |
|  10016 | 1961-05-02 | Kazuhito   | Cappelletti | M      | 1995-01-27 |
|  10017 | 1958-07-06 | Cristinel  | Bouloucos   | F      | 1993-08-03 |
+--------+------------+------------+-------------+--------+------------+
5 rows in set (0.01 sec)
```

**選取出生年份 >= 1960 且性別為 'F' 的所有紀錄的 first_name 與 last_name**

```shell
mysql> SELECT first_name, last_name FROM employees WHERE year(birth_date) >= 1960 AND gender='F' LIMIT 5;
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| Bezalel    | Simmel    |
| Duangkaew  | Piveteau  |
| Divier     | Reistad   |
| Jeong      | Reistad   |
| Mingsen    | Casley    |
+------------+-----------+
5 rows in set (0.00 sec)
```

**顯示紀錄總數**

```shell
mysql> SELECT COUNT(*) FROM employees;
+----------+
| COUNT(*) |
+----------+
|   300024 |
+----------+
1 row in set (0.05 sec)
```

**顯示各性別的紀錄數量**

```shell
mysql> SELECT gender, COUNT(*) FROM employees GROUP BY gender;
+--------+----------+
| gender | COUNT(*) |
+--------+----------+
| M      |   179973 |
| F      |   120051 |
+--------+----------+
2 rows in set (0.14 sec)
```

**按 hire_date 年份分組，並顯示該年僱用人數，且只顯示僱用超過 2 萬人的年份**

```shell
mysql> SELECT year(hire_date), COUNT(*) FROM employees GROUP BY year(hire_date) HAVING COUNT(*) > 20000;
+-----------------+----------+
| year(hire_date) | COUNT(*) |
+-----------------+----------+
|            1985 |    35316 |
|            1986 |    36150 |
|            1987 |    33501 |
|            1988 |    31436 |
|            1989 |    28394 |
|            1990 |    25610 |
|            1991 |    22568 |
|            1992 |    20402 |
+-----------------+----------+
8 rows in set (0.14 sec)
```

**依 hire_date 由大到小排序，hire_date 相同則依 birth_date 由小到大排序，顯示所有紀錄**

```shell
mysql> SELECT * FROM employees ORDER BY hire_date DESC, birth_date ASC LIMIT 5;
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
| 463807 | 1964-06-12 | Bikash     | Covnot    | M      | 2000-01-28 |
| 428377 | 1957-05-09 | Yucai      | Gerlach   | M      | 2000-01-23 |
| 499553 | 1954-05-06 | Hideyuki   | Delgrande | F      | 2000-01-22 |
| 222965 | 1959-08-07 | Volkmar    | Perko     | F      | 2000-01-13 |
|  47291 | 1960-09-09 | Ulf        | Flexer    | M      | 2000-01-12 |
+--------+------------+------------+-----------+--------+------------+
5 rows in set (0.12 sec)
```

### SELECT - 聯接 (JOINS)
`JOIN` 語句用於根據特定條件，從兩個或多個資料表產生合併結果集。它也可以用於 `UPDATE` 與 `DELETE` 語句，但此處我們主要以 Select 查詢為主。

以下是 Join 的基本通用格式：

```
SELECT table1.col1, table2.col1, ... (任意組合)
FROM
table1 <join_type> table2
ON (或 USING，視 join_type 而定) table1.column_for_joining = table2.column_for_joining
WHERE …
```

可以選取任意數量的欄位，但建議只選擇相關欄位以提高結果集的可讀性。其它子句如 `WHERE`、`GROUP BY` 則非必須。

接著談談 MySQL 支援的 JOIN 類型。

**內部連接（Inner Join）**

將表 A 與表 B 根據條件連接。只會選取條件為真的紀錄。

顯示員工部分資料及其薪資：

```shell
mysql> SELECT e.emp_no,e.first_name,e.last_name,s.salary FROM employees e JOIN salaries s ON e.emp_no=s.emp_no LIMIT 5;
+--------+------------+-----------+--------+
| emp_no | first_name | last_name | salary |
+--------+------------+-----------+--------+
|  10001 | Georgi     | Facello   |  60117 |
|  10001 | Georgi     | Facello   |  62102 |
|  10001 | Georgi     | Facello   |  66074 |
|  10001 | Georgi     | Facello   |  66596 |
|  10001 | Georgi     | Facello   |  66961 |
+--------+------------+-----------+--------+
5 rows in set (0.00 sec)
```

也可以用：

```shell
mysql> SELECT e.emp_no,e.first_name,e.last_name,s.salary FROM employees e JOIN salaries s USING (emp_no) LIMIT 5;
+--------+------------+-----------+--------+
| emp_no | first_name | last_name | salary |
+--------+------------+-----------+--------+
|  10001 | Georgi     | Facello   |  60117 |
|  10001 | Georgi     | Facello   |  62102 |
|  10001 | Georgi     | Facello   |  66074 |
|  10001 | Georgi     | Facello   |  66596 |
|  10001 | Georgi     | Facello   |  66961 |
+--------+------------+-----------+--------+
5 rows in set (0.00 sec)
```

也可以用：

```shell
mysql> SELECT e.emp_no,e.first_name,e.last_name,s.salary FROM employees e NATURAL JOIN salaries s LIMIT 5;
+--------+------------+-----------+--------+
| emp_no | first_name | last_name | salary |
+--------+------------+-----------+--------+
|  10001 | Georgi     | Facello   |  60117 |
|  10001 | Georgi     | Facello   |  62102 |
|  10001 | Georgi     | Facello   |  66074 |
|  10001 | Georgi     | Facello   |  66596 |
|  10001 | Georgi     | Facello   |  66961 |
+--------+------------+-----------+--------+
5 rows in set (0.00 sec)
```

**外部連接（Outer Join）**

主要有兩種：

- **LEFT** - 以表 A 為主，選取表 A 的全部紀錄，而表 B 只會選取符合連接條件的紀錄。
- **RIGHT** - 與 `LEFT JOIN` 相反。

假設以下兩個範例表格，以利理解 `LEFT JOIN`：

```shell
mysql> SELECT * FROM dummy1;
+----------+------------+
| same_col | diff_col_1 |
+----------+------------+
|        1 | A          |
|        2 | B          |
|        3 | C          |
+----------+------------+

mysql> SELECT * FROM dummy2;
+----------+------------+
| same_col | diff_col_2 |
+----------+------------+
|        1 | X          |
|        3 | Y          |
+----------+------------+
```

一般的 `SELECT JOIN` 如下：

```shell
mysql> SELECT * FROM dummy1 d1 LEFT JOIN dummy2 d2 ON d1.same_col=d2.same_col;
+----------+------------+----------+------------+
| same_col | diff_col_1 | same_col | diff_col_2 |
+----------+------------+----------+------------+
|        1 | A          |        1 | X          |
|        3 | C          |        3 | Y          |
|        2 | B          |     NULL | NULL       |
+----------+------------+----------+------------+
3 rows in set (0.00 sec)
```

可以寫成：

```shell
mysql> SELECT * FROM dummy1 d1 LEFT JOIN dummy2 d2 USING(same_col);
+----------+------------+------------+
| same_col | diff_col_1 | diff_col_2 |
+----------+------------+------------+
|        1 | A          | X          |
|        3 | C          | Y          |
|        2 | B          | NULL       |
+----------+------------+------------+
3 rows in set (0.00 sec)
```

也可以寫成：

```shell
mysql> SELECT * FROM dummy1 d1 NATURAL LEFT JOIN dummy2 d2;
+----------+------------+------------+
| same_col | diff_col_1 | diff_col_2 |
+----------+------------+------------+
|        1 | A          | X          |
|        3 | C          | Y          |
|        2 | B          | NULL       |
+----------+------------+------------+
3 rows in set (0.00 sec)
```

**交叉連接（Cross Join）**

不帶條件，對表 A 與表 B 做笛卡爾積，現實應用中較少用到。

簡單的 `CROSS JOIN` 如下：

```shell
mysql> SELECT * FROM dummy1 CROSS JOIN dummy2;
+----------+------------+----------+------------+
| same_col | diff_col_1 | same_col | diff_col_2 |
+----------+------------+----------+------------+
|        1 | A          |        3 | Y          |
|        1 | A          |        1 | X          |
|        2 | B          |        3 | Y          |
|        2 | B          |        1 | X          |
|        3 | C          |        3 | Y          |
|        3 | C          |        1 | X          |
+----------+------------+----------+------------+
6 rows in set (0.01 sec)
```

一個實用的例子是當你要填補缺少的項目。例如將 `dummy1` 資料全部插入另一個表 `dummy3`，每筆紀錄需有三個狀態（1、5、7）的項目。

```shell
mysql> DESC dummy3;
+----------+----------+------+-----+---------+-------+
| Field    | Type     | Null | Key | Default | Extra |
+----------+----------+------+-----+---------+-------+
| same_col | int      | YES  |     | NULL    |       |
| value    | char(15) | YES  |     | NULL    |       |
| status   | smallint | YES  |     | NULL    |       |
+----------+----------+------+-----+---------+-------+
3 rows in set (0.02 sec)
```

你可以自行寫很多筆 INSERT，或用 `CROSS JOIN` 產生所需的結果集。

```shell
mysql> SELECT * FROM dummy1 
CROSS JOIN 
(SELECT 1 UNION SELECT 5 UNION SELECT 7) T2 
ORDER BY same_col;
+----------+------------+---+
| same_col | diff_col_1 | 1 |
+----------+------------+---+
|        1 | A          | 1 |
|        1 | A          | 5 |
|        1 | A          | 7 |
|        2 | B          | 1 |
|        2 | B          | 5 |
|        2 | B          | 7 |
|        3 | C          | 1 |
|        3 | C          | 5 |
|        3 | C          | 7 |
+----------+------------+---+
9 rows in set (0.00 sec)
```

以上查詢中的 **T2** 稱為 *子查詢*，下一節將會解說。

**自然連接（Natural Join）**

自動選取表 A 與表 B 的共同欄位，並執行內部連接。

```shell
mysql> SELECT e.emp_no,e.first_name,e.last_name,s.salary FROM employees e NATURAL JOIN salaries s LIMIT 5;
+--------+------------+-----------+--------+
| emp_no | first_name | last_name | salary |
+--------+------------+-----------+--------+
|  10001 | Georgi     | Facello   |  60117 |
|  10001 | Georgi     | Facello   |  62102 |
|  10001 | Georgi     | Facello   |  66074 |
|  10001 | Georgi     | Facello   |  66596 |
|  10001 | Georgi     | Facello   |  66961 |
+--------+------------+-----------+--------+
5 rows in set (0.00 sec)
```

注意到 `NATURAL JOIN` 與使用 `USING`，會自動在結果集只顯示共同欄位一次（若未明確指定欄位）。

**更多範例**

顯示薪資超過 80000 員工的 emp_no、salary、title 及所屬部門 dept：

```shell
mysql> SELECT e.emp_no, s.salary, t.title, d.dept_no 
FROM  
employees e 
JOIN salaries s USING (emp_no) 
JOIN titles t USING (emp_no) 
JOIN dept_emp d USING (emp_no) 
WHERE s.salary > 80000 
LIMIT 5;
+--------+--------+--------------+---------+
| emp_no | salary | title        | dept_no |
+--------+--------+--------------+---------+
|  10017 |  82163 | Senior Staff | d001    |
|  10017 |  86157 | Senior Staff | d001    |
|  10017 |  89619 | Senior Staff | d001    |
|  10017 |  91985 | Senior Staff | d001    |
|  10017 |  96122 | Senior Staff | d001    |
+--------+--------+--------------+---------+
5 rows in set (0.00 sec)
```

按部門與職稱統計員工數量，並依部門編號排序：

```shell
mysql> SELECT d.dept_no, t.title, COUNT(*) 
FROM titles t 
LEFT JOIN dept_emp d USING (emp_no) 
GROUP BY d.dept_no, t.title 
ORDER BY d.dept_no 
LIMIT 10;
+---------+--------------------+----------+
| dept_no | title              | COUNT(*) |
+---------+--------------------+----------+
| d001    | Manager            |        2 |
| d001    | Senior Staff       |    13940 |
| d001    | Staff              |    16196 |
| d002    | Manager            |        2 |
| d002    | Senior Staff       |    12139 |
| d002    | Staff              |    13929 |
| d003    | Manager            |        2 |
| d003    | Senior Staff       |    12274 |
| d003    | Staff              |    14342 |
| d004    | Assistant Engineer |     6445 |
+---------+--------------------+----------+
10 rows in set (1.32 sec)
```

#### SELECT - 子查詢 (Subquery)
子查詢通常是較小的結果集，可用於驅動外層 `SELECT` 的多種操作。  
可用於 `WHERE` 條件中，或用以替代 JOIN，尤其是在 JOIN 過於複雜時。  
這些子查詢亦稱為衍生表（derived tables），必須在 SELECT 查詢中帶有資料表別名。

以下來看一些子查詢範例。

以下範例透過子查詢從 `departments` 表取得部門名稱，子查詢裡使用 `dept_emp` 表的 `dept_no`：

```shell
mysql> SELECT e.emp_no, 
(SELECT dept_name FROM departments WHERE dept_no=d.dept_no) dept_name FROM employees e 
JOIN dept_emp d USING (emp_no) 
LIMIT 5;
+--------+-----------------+
| emp_no | dept_name       |
+--------+-----------------+
|  10001 | Development     |
|  10002 | Sales           |
|  10003 | Production      |
|  10004 | Production      |
|  10005 | Human Resources |
+--------+-----------------+
5 rows in set (0.01 sec)
```

接著用前面查詢到的薪資平均值作為子查詢，列出員工中的最新薪資高於平均薪資者：

```shell
mysql> SELECT AVG(salary) FROM salaries;
+-------------+
| AVG(salary) |
+-------------+
|  63810.7448 |
+-------------+
1 row in set (0.80 sec)

mysql> SELECT e.emp_no, MAX(s.salary) 
FROM employees e 
NATURAL JOIN salaries s 
GROUP BY e.emp_no 
HAVING MAX(s.salary) > (SELECT AVG(salary) FROM salaries) 
LIMIT 10;
+--------+---------------+
| emp_no | MAX(s.salary) |
+--------+---------------+
|  10001 |         88958 |
|  10002 |         72527 |
|  10004 |         74057 |
|  10005 |         94692 |
|  10007 |         88070 |
|  10009 |         94443 |
|  10010 |         80324 |
|  10013 |         68901 |
|  10016 |         77935 |
|  10017 |         99651 |
+--------+---------------+
10 rows in set (0.56 sec)
```

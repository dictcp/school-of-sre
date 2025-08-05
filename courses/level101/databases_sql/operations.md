*   解釋及解釋+分析

	`EXPLAIN <query>` 會分析優化器的查詢計畫，包括表格如何連接、哪些表格/列被掃描等。

	`EXPLAIN ANALYZE` 除了上述內容外，還會顯示執行成本、返回的列數、所花時間等額外資訊。

	這些知識對調整查詢和添加索引非常有用。

	觀看這個效能調校的 [教學影片](https://www.youtube.com/watch?v=pjRTLPeUOug)。

	查看 [實作練習區](https://dictcp.github.io/school-of-sre/level101/databases_sql/lab/) 以進行索引相關的實作。

*   [慢查詢日誌](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)

	用於識別慢查詢（閾值可配置），可在配置文件中啟用，也能透過查詢動態啟用。

	查看關於辨識慢查詢的 [實作練習區](https://dictcp.github.io/school-of-sre/level101/databases_sql/lab/)。

*   使用者管理

	包含使用者的建立與變更，例如管理權限、變更密碼等。

*   備份與還原策略、優缺點

	- 使用 `mysqldump` 的邏輯備份 — 較慢但可以線上執行

	- 實體備份（複製資料目錄或使用 XtraBackup） — 快速備份/還原。複製資料目錄需要鎖定或關閉服務。XtraBackup 改善了這一點，因為它支持不中斷服務的熱備份。

	- 其他方法 — 如時間點還原 (PITR)、快照等。

*   使用重做日誌的故障恢復流程

	當崩潰後重新啟動伺服器時，會讀取重做日誌並重播修改以進行恢復。

*   監控 MySQL

	- 主要 MySQL 指標：讀取、寫入、查詢運行時間、錯誤、慢查詢、連線數、執行中執行緒、InnoDB 指標

	- 主要作業系統指標：CPU、負載、記憶體、磁碟 I/O、網路

*   複寫 (Replication)

    將資料從一個實例複製到一個或多個實例。協助水平擴展、資料保護、分析及效能提升。主節點有 binlog 傳輸執行緒，從節點則有複寫 I/O 與 SQL 執行緒。策略包含標準非同步、半同步及群組複寫。

*   高可用性 (High Availability)

    能夠應對軟體、硬體及網路層級的故障。對需要 99.9% 以上正常運作時間者不可或缺。可透過 MySQL、Percona、Oracle 等提供的複寫或叢集解決方案實現。設置與維護需要專業技能。故障轉移可以是手動、腳本化或使用 Orchestrator 等工具。

*   [資料目錄](https://dev.mysql.com/doc/refman/8.0/en/data-directory.html)

    資料存放在特定目錄中，每個資料庫都有對應的子目錄。此外還有 MySQL 日誌檔、InnoDB 日誌檔、伺服器進程 ID 檔及其他配置。資料目錄是可設定的。

*   [MySQL 配置](https://dev.mysql.com/doc/refman/5.7/en/server-configuration.html)

    可以透過啟動時帶入的 [參數](https://dev.mysql.com/doc/refman/5.7/en/server-options.html) 或是 [配置檔](https://dev.mysql.com/doc/refman/8.0/en/option-files.html) 來設定。MySQL 會在幾個 [標準路徑](https://dev.mysql.com/doc/refman/8.0/en/option-files.html#option-file-order) 中尋找配置檔，`/etc/my.cnf` 是常用路徑之一。這些選項以標頭組織（伺服器部分用 `mysqld`、用戶端部分用 `mysql`），可透過後續實作練習更深入探索。

*   [日誌](https://dev.mysql.com/doc/refman/5.7/en/server-logs.html)

    MySQL 有多種用途的日誌—一般查詢日誌、錯誤日誌、二進位日誌（用於複寫）、慢查詢日誌。預設只啟用錯誤日誌（降低 I/O 與存儲需求），其他日誌可視需要啟用，方法是啟動時指定配置參數或執行時透過指令。也可利用配置參數調整 [日誌輸出目的地](https://dev.mysql.com/doc/refman/5.7/en/log-destinations.html)。

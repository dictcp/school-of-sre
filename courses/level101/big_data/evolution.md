# Hadoop 的演進

![Evolution of hadoop](images/hadoop_evolution.png)

# Hadoop 的架構

1. **HDFS**
    1. Hadoop 分散式檔案系統（HDFS）是一個設計用於在普通硬體上運行的分散式檔案系統。它與現有的分散式檔案系統有許多相似之處，不過與其他分散式檔案系統的差異也相當明顯。
    2. HDFS 具高度容錯能力，並設計為能部署於低成本硬體。HDFS 提供對應用程式資料的高吞吐量訪問，適合具有大規模資料集的應用。
    3. HDFS 是 [Apache Hadoop Core 專案](https://github.com/apache/hadoop) 的一部分。

    ![HDFS 架構](images/hdfs_architecture.png)

    HDFS 的主要組件包括：
    1. NameNode：為集群中檔案命名空間的仲裁者與中央儲存庫。NameNode 處理打開、關閉及重命名檔案和目錄等操作。
    2. DataNode：管理附加於該節點的儲存空間，負責處理所有讀寫請求。根據 NameNode 指令執行操作，如區塊的建立、刪除和複製。
    3. Client：負責從 NameNode 取得所需的元資料，接著與 DataNode 溝通以完成讀取和寫入作業。 </br></br></br>

2. **YARN**
    YARN 是 “Yet Another Resource Negotiator”（另一個資源協調器）的縮寫。它在 Hadoop 2.0 中引入，以解決 Hadoop 1.0 中 Job Tracker 的瓶頸問題。當時 YARN 被描述為“重新設計的資源管理器”，目前已演變為用於大數據處理的大規模分散式作業系統。

    ![YARN 架構](images/yarn_architecture.gif)
    
    YARN 架構的主要組件包括：
    1. Client：提交 MapReduce（MR）工作給資源管理器。
    2. Resource Manager：YARN 的主控守護程序，負責所有應用的資源分配與管理。當接收處理請求時，會將其轉送給相應的節點管理器，並依需求分配資源。其內含兩大組件：
        1. Scheduler：根據已分配的應用和可用資源進行排程。它純粹是排程器，無監控或追蹤功能，也無保證失敗任務會重啟。YARN 排程器支持 Capacity Scheduler 和 Fair Scheduler 等插件以分割叢集資源。
        2. Application manager：負責接受應用並從資源管理器協商第一個容器。任務失敗時，亦會重啟 Application Manager 容器。
    3. Node Manager：管理 Hadoop 叢集中個別節點和該節點上的應用程式及工作流程。主要工作是與 Node Manager 保持同步，監控資源使用，管理日誌，並依資源管理器指示終止容器。它也負責建立並啟動容器進程，根據 Application Master 的請求。
    4. Application Master：應用程序是提交給框架的單一工作。Application Master 負責與資源管理器協商資源，追蹤狀態並監控單一應用的進展。它透過送出 Container Launch Context (CLC) 向 Node Manager 請求容器，該 CLC 包括應用執行所需所有資訊。應用啟動後，會不定時向資源管理器報告狀態。
    5. Container：為單一節點上的物理資源集合，如 RAM、CPU 核心及磁碟。容器由 Container Launch Context (CLC) 呼叫，CLC 包含環境變數、權限令牌、依賴程式等資訊。 </br></br>


# MapReduce 框架

![MapReduce Framework](images/map_reduce.jpg)

1. MapReduce 一詞代表 Hadoop 程式所執行的兩個不同任務——Map 工作與 Reduce 工作。Map 工作以資料集為輸入並處理產生鍵值對；Reduce 工作則接受 Map 工作的輸出（鍵值對）進行聚合以產生所需結果。
2. Hadoop MapReduce（Hadoop Map/Reduce）是一個適用於計算叢集的大規模資料分散式處理軟體框架。MapReduce 協助將輸入資料集拆分成多個部分，並能同時平行執行對各部分資料的程式。
3. 以下為 Word Count 範例，展示 MapReduce 框架的使用：

![Word Count Example](images/mapreduce_example.jpg)
</br></br>

# Hadoop 周邊其他工具

1. [**Hive**](https://hive.apache.org/)
    1. 使用稱為 HQL 的語言，極為類似 SQL。賦予非程式設計師查詢與分析 Hadoop 資料的能力，基本上是建立在 MapReduce 之上的抽象層。
    2. 例如 HQL 查詢：
        1. `SELECT pet.name, comment FROM pet JOIN event ON (pet.name = event.name);`
    3. 在 MySQL 中：
        1. `SELECT pet.name, comment FROM pet, event WHERE pet.name = event.name;`
2. [**Pig**](https://pig.apache.org/)
    1. 使用稱為 Pig Latin 的指令語言，強調工作流程導向。不需成為 Java 專家，但需具備少許編碼能力。也是建立在 MapReduce 之上的抽象層。
    2. 這裡有個小問題給你：
    執行下圖中右欄 Pig 查詢語句，針對左欄的資料，输出結果為何？

    ![Pig Example](images/pig_example.png)

    輸出：
    <pre><code>
    7,Komal,Nayak,24,9848022334,trivendram
    8,Bharathi,Nambiayar,24,9848022333,Chennai
    5,Trupthi,Mohanthy,23,9848022336,Bhuwaneshwar
    6,Archana,Mishra,23,9848022335,Chennai
    </code></pre>

3. [**Spark**](https://spark.apache.org/)
    1. Spark 提供基礎用於叢集中的記憶體運算，使使用者程式可將資料載入叢集記憶體並反覆查詢，非常適合機器學習演算法。
4. [**Presto**](https://prestodb.io/)
    1. Presto 是一個高效能、分散式的大數據 SQL 查詢引擎。
    2. 其架構允許使用者查詢多種資料來源，包含 Hadoop、AWS S3、Alluxio、MySQL、Cassandra、Kafka 及 MongoDB。
    3. 範例 Presto 查詢：
    <pre><code>
    USE studentDB;
    SHOW TABLES;
    SELECT roll_no, name FROM studentDB.studentDetails WHERE section=’A’ LIMIT 5;
    </code></pre>   
    
</br>

# 資料序列化與儲存

1. 為了透過網路傳輸資料或存放於永久儲存裝置，需要將資料結構或物件狀態轉換為二進位或文字形式的過程，我們稱之為序列化。
2. Avro 資料存放於容器檔案（一個 `.avro` 檔案），其結構描述檔（`.avsc` 檔案）與資料檔一起儲存。
3. Apache Hive 支援將資料表以 Avro 形式儲存，並可針對此序列化格式的資料進行查詢。

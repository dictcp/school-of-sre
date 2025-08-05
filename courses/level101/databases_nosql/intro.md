# NoSQL 概念

## 預備知識
- [關聯式資料庫](https://linkedin.github.io/school-of-sre/level101/databases_sql/intro/)

## 課程預期

完成此培訓後，您將了解什麼是 NoSQL 資料庫，NoSQL 相較於傳統關聯式資料庫的優缺點，學習不同類型的 NoSQL 資料庫，並理解 NoSQL 背後的一些基本概念與權衡。

## 課程不涵蓋內容

本課程不會深入探討任何特定的 NoSQL 資料庫。

## 課程內容

*   [NoSQL 簡介](https://linkedin.github.io/school-of-sre/level101/databases_nosql/intro/#introduction)
*   [CAP 理論](https://linkedin.github.io/school-of-sre/level101/databases_nosql/key_concepts/#cap-theorem)
*   [資料版本控制](https://linkedin.github.io/school-of-sre/level101/databases_nosql/key_concepts/#versioning-of-data-in-distributed-systems)
*   [分區 (Partitioning)](https://linkedin.github.io/school-of-sre/level101/databases_nosql/key_concepts/#partitioning)
*   [雜湊 (Hashing)](https://linkedin.github.io/school-of-sre/level101/databases_nosql/key_concepts/#hashing)
*   [法定人數 (Quorum)](https://linkedin.github.io/school-of-sre/level101/databases_nosql/key_concepts/#quorum)

## 介紹

當人們提到「NoSQL 資料庫」，通常指非關聯式資料庫。有些人認為 NoSQL 代表「non SQL（非 SQL）」而有些人則說它代表「not only SQL（不僅是 SQL）」。無論如何，多數人同意 NoSQL 資料庫是以非關聯式表格外的格式來存儲資料的資料庫。

一個常見誤解是 NoSQL 或非關聯式資料庫不擅長存儲關聯資料，但實際上 NoSQL 資料庫可以存儲關聯資料—只是存法不同於關聯式資料庫。事實上，與 SQL 資料庫相比，許多人覺得在 NoSQL 資料庫中建模關聯資料更「容易」，因為相關資料不必拆分到多個表格中。

此類資料庫自 1960 年代末即已存在，但「NoSQL」此名稱直到 21 世紀初才被提出。NASA 曾使用 NoSQL 資料庫來追蹤阿波羅任務的設備庫存。隨著 2000 年代末存儲成本大幅下降，NoSQL 資料庫開始崛起。過去為了避免資料重複，常需要創建複雜難管的資料模型，如今則不需要了。軟體開發中主要成本漸從存儲轉向開發者，NoSQL 資料庫因而優化了開發者效率。隨著敏捷開發方法的興起，NoSQL 資料庫專注於可擴展性、高速性能，同時允許頻繁的應用程式變更並簡化程式設計。

### NoSQL 資料庫類型

隨著不同公司需求的多樣化，NoSQL 資料庫發展出了多種類型，但可大致分為以下 4 種，有些資料庫類型間會有重疊：

1. **文件型資料庫 (Document databases)：**  
    以類似 [JSON](https://www.json.org/json-en.html)（JavaScript 物件表示法）的文件存儲資料。每個文件包含欄位和值的鍵值對，值常見類型包括字串、數字、布林陣列或物件，其結構通常與程式碼中的物件對應。優點是直覺的数据模型與靈活的架構。由於支持多種欄位值類型及強大查詢語言，文件型資料庫適合多種用例，可用作通用資料庫，且可水平擴展以支持大量資料。例子：MongoDB、Couchbase。

2. **鍵值資料庫 (Key-Value databases)：**  
    簡單的資料庫類型，每筆資料包含鍵和值。值通常只能藉由鍵查詢，查詢鍵值對的方式簡單。適合大量資料存儲但不需複雜查詢的使用場景。常見用途包括存儲用戶偏好設定或快取。例子：[Redis](https://redis.io/)、[DynamoDB](https://aws.amazon.com/dynamodb/)、[Voldemort](https://www.project-voldemort.com/voldemort/) / [Venice](https://engineering.linkedin.com/blog/2017/04/building-venice--a-production-software-case-study)（LinkedIn）。

3. **寬欄位儲存 (Wide-Column stores)：**  
    以資料表、列與動態欄位存儲資料。相較關聯式資料庫，寬欄位儲存提供高度彈性，因為每列不必包含相同欄位。許多人視其為二維鍵值資料庫。適合儲存大量資料並能預測查詢模式的需求。常用於物聯網資料與使用者資料。代表資料庫有 [Cassandra](https://cassandra.apache.org/) 與 [HBase](https://hbase.apache.org/)。

4. **圖形資料庫 (Graph Databases)：**  
    以節點與邊來存儲資料。節點通常用於存放人、地點或事物資訊，邊則存描述節點間關聯。圖形資料庫底層存儲方式各異，部分依賴關聯引擎並在表格中「儲存」圖形資料（此為邏輯抽象層），其他則使用鍵值庫或文件導向資料庫，天然屬於 NoSQL 結構。圖形資料庫擅長於需要遍歷關聯以尋找模式的應用，如社交網絡、詐欺偵測和推薦引擎。例子：[Neo4j](https://neo4j.com/)

### **比較**

<table>
  <tr>
   <td>
   </td>
   <td>效能
   </td>
   <td>可擴展性
   </td>
   <td>彈性
   </td>
   <td>複雜度
   </td>
   <td>功能性
   </td>
  </tr>
  <tr>
   <td>鍵值型
   </td>
   <td>高
   </td>
   <td>高
   </td>
   <td>高
   </td>
   <td>無
   </td>
   <td>可變
   </td>
  </tr>
  <tr>
   <td>文件型
   </td>
   <td>高
   </td>
   <td>可變（高）
   </td>
   <td>高
   </td>
   <td>低
   </td>
   <td>可變（低）
   </td>
  </tr>
  <tr>
   <td>寬欄位
   </td>
   <td>高
   </td>
   <td>高
   </td>
   <td>中等
   </td>
   <td>低
   </td>
   <td>最少
   </td>
  </tr>
  <tr>
   <td>圖形
   </td>
   <td>可變
   </td>
   <td>可變
   </td>
   <td>高
   </td>
   <td>高
   </td>
   <td>圖論
   </td>
  </tr>
</table>

### SQL 與 NoSQL 差異比較

下表彙整了 SQL 與 NoSQL 資料庫的主要差異。

<table>
  <tr>
   <td>
   </td>
   <td>SQL 資料庫
   </td>
   <td>NoSQL 資料庫
   </td>
  </tr>
  <tr>
   <td>資料存儲模型
   </td>
   <td>固定列與欄的表格
   </td>
   <td>文件：JSON 文件，鍵值：鍵值對，寬欄位：具動態欄位的表格，圖形：節點與邊
   </td>
  </tr>
  <tr>
   <td>主要用途
   </td>
   <td>通用目的
   </td>
   <td>文件：通用目的，鍵值：大量簡單查詢資料，寬欄位：大量且可預測查詢資料，圖形：分析遍歷資料關聯
   </td>
  </tr>
  <tr>
   <td>架構規範
   </td>
   <td>嚴格
   </td>
   <td>彈性
   </td>
  </tr>
  <tr>
   <td>擴展方式
   </td>
   <td>垂直擴展（使用更強大的伺服器）
   </td>
   <td>水平擴展（跨平價伺服器擴展）
   </td>
  </tr>
  <tr>
   <td>多筆資料 <a href="https://en.wikipedia.org/wiki/ACID">ACID </a>交易
   </td>
   <td>支持
   </td>
   <td>多數不支持多筆 ACID 交易，部分如 MongoDB 支持
   </td>
  </tr>
  <tr>
   <td>聯結 (Joins)
   </td>
   <td>通常需要
   </td>
   <td>通常不需要
   </td>
  </tr>
  <tr>
   <td>資料與物件映射
   </td>
   <td>需要 ORM (物件關聯映射)
   </td>
   <td>多數不需 ORM，文件型資料庫文件可直接映射至主流程式語言中的資料結構
   </td>
  </tr>
</table>

### 優點

*   **靈活的資料模型**

    多數 NoSQL 系統擁有彈性架構，允許您輕鬆修改資料庫結構以新增或刪除欄位，支援應用程式不斷演進的需求。促進持續開發新功能而不需繁重的資料庫操作工作。

*   **水平擴展**

    多數 NoSQL 系統支持水平擴展，即可隨時加入較便宜且通用的硬體以擴充系統。反觀 SQL 系統多數採垂直擴展（升級更強伺服器）。與傳統 SQL 系統相比，NoSQL 系統可處理更大量的資料。

*   **快速查詢**

    由於資料非正規化與支持水平擴展，NoSQL 通常比傳統 SQL 更快。多數 NoSQL 系統還傾向將相似資料聚集，有助於加速查詢回應。

*   **提升開發者生產力**

    NoSQL 系統的資料映射通常根據程式設計中的資料結構，使開發者所需的資料轉換較少，進而提高生產力並減少錯誤。

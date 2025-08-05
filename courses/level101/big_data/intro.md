# 大數據

## 前置知識

- Linux 檔案系統的基礎。
- 系統設計的基本了解。

## 課程期望

本課程涵蓋大數據的基礎知識及其演進歷程。我們將探討幾個現實場景，說明大數據如何成為這些場景的完美解決方案。課程中會有一項關於設計大數據系統的有趣作業，之後將介紹 Hadoop 的架構及其相關工具。

## 課程不涵蓋內容

撰寫程式以從資料中提取分析。

## 課程內容

1. [大數據概覽](https://dictcp.github.io/school-of-sre/level101/big_data/intro/#overview-of-big-data)
2. [大數據技術的應用](https://dictcp.github.io/school-of-sre/level101/big_data/intro/#usage-of-big-data-techniques)
3. [Hadoop 的演進](https://dictcp.github.io/school-of-sre/level101/big_data/evolution/)
4. [Hadoop 架構](https://dictcp.github.io/school-of-sre/level101/big_data/evolution/#architecture-of-hadoop)
    1. HDFS
    2. Yarn
5. [MapReduce 框架](https://dictcp.github.io/school-of-sre/level101/big_data/evolution/#mapreduce-framework)
6. [Hadoop 相關工具](https://dictcp.github.io/school-of-sre/level101/big_data/evolution/#other-tooling-around-hadoop)
    1. Hive
    2. Pig
    3. Spark
    4. Presto
7. [資料序列化與儲存](https://dictcp.github.io/school-of-sre/level101/big_data/evolution/#data-serialisation-and-storage)


# 大數據概覽

1. 大數據是一個龐大資料集的集合，無法使用傳統計算技術處理。它不是單一技術或工具，而是一門完整的學科，涉及各種工具、技術與框架。
2. 大數據可能包含：
    1. 結構化資料
    2. 非結構化資料
    3. 半結構化資料
3. 大數據的特性：
    1. 資料量（Volume）
    2. 種類多樣性（Variety）
    3. 資料流速（Velocity）
    4. 變異性（Variability）
4. 大數據產生的例子包括股票交易所、社交媒體網站、噴射引擎等。


# 大數據技術的應用

1. 以紅綠燈問題為例：
    1. 截至2018年，美國有超過30萬個紅綠燈。
    2. 假設我們在每個紅綠燈上安裝一個裝置來收集指標並發送至中央指標收集系統。
    3. 若每個物聯網裝置每分鐘發送10個事件，則每天我們會收到 `300000 x 10 x 60 x 24 = 432 x 10 ^ 7` 個事件。
    4. 你會如何處理這些資料，並告訴我在特定某天上午10:45有多少信號是「綠燈」？
2. 再以統一支付介面（UPI）交易為例：
    1. 2019年10月，印度約有11.5億筆UPI交易。
    2. 若將此資料推估至一整年，並想找出某個特定UPI ID經常使用的付款方式，你建議如何進行？

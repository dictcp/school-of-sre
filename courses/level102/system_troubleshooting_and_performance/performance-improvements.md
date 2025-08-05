Performance 工具有著高度重要性，是開發/運維生命周期中不可或缺的一部分，能幫助理解應用程式行為。SRE 通常會使用這些工具來評估服務的性能並相應地提出改進建議。

### Performance 分析指令

以下大多數指令對於系統或服務的性能分析是必須要知道的。

- top -：顯示系統、程序、執行緒等的即時狀況。
- htop -：與 top 命令類似，但操作介面更為互動。
- iotop -：互動式磁碟 I/O 監控工具。
- vmstat -：虛擬記憶體統計資料瀏覽器。
- iostat -：裝置和分區的輸入/輸出統計監控工具。
- free -：顯示實體記憶體和交換記憶體資訊。
- sar -：系統活動報告，提供 CPU、磁碟、記憶體、網路等多項指標。
- mpstat -：顯示 CPU 利用率及性能資訊。
- lsof -：列出開啟的文件及其對應的進程資訊。
- perf -：性能分析工具。

### Profiling 工具

Profiling 是服務性能分析中的重要環節。市面上有各式各樣的 profiler 工具，能幫助找出最常用的程式路徑、除錯、記憶體分析等，並可生成熱度圖以了解程式在負載下的效能。

- [FlameGraph](https://github.com/brendangregg/FlameGraph)：Flame graphs 是對已分析軟體的視覺化呈現，可以快速且精確地識別最頻繁的程式路徑。
- [Valgrind](https://valgrind.org/info/about.html)：程式設計工具，用於記憶體除錯、記憶體洩漏檢測及分析。
- [Gprof](https://sourceware.org/binutils/docs/gprof)：GNU 分析工具，採用插裝和取樣混合技術，插裝用於收集函式呼叫資訊，取樣用於收集執行時分析資料。

若想了解 LinkedIn 如何針對其服務執行即時 Profiling，可參考 LinkedIn 部落格文章：[ODP: An Infrastructure for On-Demand Service Profiling](https://engineering.linkedin.com/blog/2017/01/odp--an-infrastructure-for-on-demand-service-profiling)

### 基準測試（Benchmarking）

基準測試是測量服務最佳性能的過程，例如服務能承載多少 QPS、隨負載增加的延遲、主機資源利用率、loadavg 等等。回歸測試（即負載測試）是部署服務到生產環境前的必做步驟。

**部分知名工具 -：**

- [Apache Benchmark Tool, ab](https://httpd.apache.org/docs/2.4/programs/ab.html)：模擬對 Web 應用的高負載並收集分析資料。
- [Httperf](https://github.com/httperf/httperf)：以指定頻率對 Web 伺服器發送請求並收集統計，持續增加負載直到達飽和點。
- [Apache JMeter](https://github.com/apache/jmeter)：熱門開源工具，用於測量 Web 應用性能。JMeter 是基於 Java 的應用程式，不僅能測試 Web 伺服器，也可用於 PHP、Java、REST 等。
- [Wrk](https://github.com/wg/wrk)：現代化性能測試工具，對 Web 伺服器施加負載，提供延遲、每秒請求數、每秒傳輸數據等資訊。
- [Locust](https://github.com/locustio/locust)：易用、可編寫腳本且可擴展的性能測試工具。

**限制 -：**

上述工具主要用於合成負載或壓力測試，無法量測實際用戶體驗。它們無法偵測使用者端資源如何影響應用表現，例如記憶體不足、CPU 限制或網路不佳。

若想了解 LinkedIn 如何在其整體系統中執行負載測試，請參考：[Eliminating toil with fully automated load testing](https://engineering.linkedin.com/blog/2019/eliminating-toil-with-fully-automated-load-testing)

另外，若想了解 LinkedIn 如何使用即時監控（RUM）資料克服負載測試的限制並提升使用者整體體驗，請參考：[Monitor and Improve Web Performance Using RUM Data Visualization](https://engineering.linkedin.com/performance/monitor-and-improve-web-performance-using-rum-data-visualization)

### Scaling（擴展）

系統在資源可用性範圍內只能發揮至一定性能，持續優化是保障資源極限充分利用的關鍵。隨著 QPS 增加，系統需要擴展，可分為垂直擴展與水平擴展。垂直擴展有限制（CPU、記憶體、硬碟、GPU 等資源只能提升到一定程度），而水平擴展因應用設計與環境性質限制，可較容易且無限地擴充。

Web 應用擴展通常需下列作法之一或多項：

- 透過增加主機降低單一伺服器負載。
- 利用負載平衡器將流量分散到多台伺服器。
- 透過資料分片並增加讀取副本來擴充資料庫。

這裡有篇好文介紹 LinkedIn 如何擴展其應用架構：[A Brief History of Scaling LinkedIn](https://engineering.linkedin.com/architecture/brief-history-scaling-linkedin)

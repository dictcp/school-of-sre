# 結論

為了維護與排除系統故障，建立一套健全的監控與警示系統是必要的。透過包含關鍵指標的儀表板，你可以在同一個地方一覽服務的效能狀況。明確定義的警示（具備合理的門檻與通知機制）能幫助你快速辨識服務基礎架構或資源飽和的任何異常。透過採取必要行動，你可以避免服務降級，並縮短服務故障的平均偵測時間（MTTD）。

除了內部監控之外，監控真實使用者體驗有助於理解用戶感受到的服務效能。因為服務中涉及許多模組，多數不在你掌控範圍內，因此實施真實使用者監控十分重要。

指標提供的是非常抽象的服務效能細節。為了更深入了解系統狀態並在事件發生時加快恢復速度，你可能需要實作觀測性的另外兩大支柱：日誌與追蹤。日誌和追蹤資料可以協助你了解導致服務失敗或降級的原因。

以下是一些學習監控與觀測性的資源：

-   [Google SRE 書籍：監控分散式系統](https://sre.google/sre-book/monitoring-distributed-systems/)

-   [Yuri Shkuro 的《精通分散式追蹤》](https://learning.oreilly.com/library/view/mastering-distributed-tracing/9781788628464/)


## 參考資料

-   [Google SRE 書籍：監控分散式系統](https://sre.google/sre-book/monitoring-distributed-systems/)

-   [Yuri Shkuro 著《精通分散式追蹤》](https://learning.oreilly.com/library/view/mastering-distributed-tracing/9781788628464/)

-   [監控與觀測性](https://copyconstruct.medium.com/monitoring-and-observability-8417d1952e1c)

-   [零答案的三支柱](https://medium.com/lightstephq/three-pillars-with-zero-answers-2a98b36358b8)

-   工程部落格：
    [LinkedIn](https://engineering.linkedin.com/blog/topic/monitoring)、
    [Grafana](https://grafana.com/blog/)、
    [Elastic.co](https://www.elastic.co/blog/)、
    [OpenTelemetry](https://medium.com/opentelemetry)

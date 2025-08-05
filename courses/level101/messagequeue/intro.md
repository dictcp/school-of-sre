# 訊息服務

## 本課程的學習目標

訓練結束後，您將了解什麼是訊息服務，學習不同類型的訊息服務實作，並理解一些基本概念與取捨。

## 本課程不涵蓋的內容

本課程不會深入探討任何特定的訊息服務。

## 課程內容

* [訊息服務介紹](https://linkedin.github.io/school-of-sre/level101/messagequeue/intro/#introduction)
* [傳遞保證](https://linkedin.github.io/school-of-sre/level101/messagequeue/key_concepts/#delivery-guarantees)
* [訊息排序與並行處理](https://linkedin.github.io/school-of-sre/level101/messagequeue/key_concepts/#messages-ordering-and-parallelism)
* [Fan Out / In](https://linkedin.github.io/school-of-sre/level101/messagequeue/key_concepts/#fan-out--in)
* [毒丸訊息與死信](https://linkedin.github.io/school-of-sre/level101/messagequeue/key_concepts/#poison-pills-and-dead-letters)

## 介紹

在現今的分散式系統與微服務架構中，訊息服務在確保各元件間可靠的通訊與協作中扮演重要角色。這些服務允許非同步訊息交換，帶來多項好處，例如提升效能、改善容錯能力與增強擴充性。

本文將概述各種可用的訊息服務類型，包括通用訊息佇列、發布/訂閱訊息、串流處理、無中介者訊息系統，以及資料庫作為佇列的系統。我們也將探討關鍵概念，如傳遞保證、訊息排序、並行處理、毒丸訊息與死信，這些都是理解訊息服務如何運作及其有效利用方式的要點。

### 訊息服務類型：

本節將介紹各種訊息服務，每種設計用於解決分散式系統中不同需求與應用場景。

1. **通用訊息佇列：** 通用訊息佇列用途廣泛，可應用於各式非特殊場景，從任務分配、請求緩衝，到微服務間通訊皆可使用。這些系統設計用以確保訊息可靠傳遞，並確保訊息按照正確順序處理，通常能處理每秒最多約 100,000 條訊息量。訊息佇列通常支持多種訊息模式，如點對點與發布/訂閱，為不同應用提供彈性。通用訊息佇列範例包括 [RabbitMQ](https://www.rabbitmq.com/)、[ActiveMQ](https://activemq.apache.org/) 與 [Amazon SQS](https://aws.amazon.com/sqs/)。透過使用這些訊息佇列，開發者可以使應用解耦並獨立擴展，提升整體系統韌性與效能。

2. **發布/訂閱 (Pub/Sub) 訊息：** 發布-訂閱訊息服務允許發布者將訊息傳送給多個訂閱者，無需一對一連線，達成生產者與消費者的解耦，使系統更具擴展性與容錯性。Pub/Sub 系統尤其適用於多個消費者需接收並處理相同訊息的場景，例如發送通知、紀錄或資料複製。此模型支持動態訂閱管理，允許消費者在執行時訂閱或取消訂閱特定主題或頻道。Pub/Sub 訊息服務範例包括 [Google Cloud Pub/Sub](https://cloud.google.com/pubsub)、[Apache Pulsar](https://pulsar.apache.org/)、[Azure Event Grid](https://azure.microsoft.com/en-us/products/event-grid)、[AWS SNS](https://aws.amazon.com/sns/) 和 [NATS](https://nats.io/)。採用 Pub/Sub 訊息系統，開發者可以構建事件驅動架構，降低系統複雜度，並簡化新服務整合。

3. **串流處理：** 串流處理服務設計用於處理大量即時資料（例如每秒百萬條訊息），允許持續資料流的處理與分析。這些服務支持複雜事件處理、時間窗格聚合及有狀態的轉換，為建立即時分析、監控與警示應用提供穩健平台。串流處理系統常結合內存與磁碟儲存，平衡效能與持久性，並支持水平擴展，使開發者能以低延遲處理龐大資料量。串流處理範例包括 [Apache Kafka](https://kafka.apache.org/)、[Amazon Kinesis Data Streams](https://aws.amazon.com/kinesis/data-streams/)、[Azure Event Hubs](https://azure.microsoft.com/en-us/products/event-hubs)、[RocketMQ](https://rocketmq.apache.org/)、[Apache Pulsar](https://pulsar.apache.org/) 與 [Redis Streams](https://redis.io/docs/data-types/streams/)。利用串流處理服務，組織能從資料中即時獲取寶貴洞見，並更有效做出資料驅動決策。

4. **無中介者 (Brokerless)：** 無中介者訊息系統允許生產者與消費者直接通訊，不需依賴中央代理，使延遲降低、效能提升。在此系統中，節點透過點對點架構相互溝通，簡化部署並減少對專用訊息代理基礎架構的需求。此系統特別適合高效能、低延遲應用，或網路連線不穩定的情況。無中介者訊息系統範例有 [ZeroMQ](https://zeromq.org/)、[nanomsg](https://nanomsg.org/)、[Chronicle Queue](https://github.com/OpenHFT/Chronicle-Queue) 與 [Disruptor](https://lmax-exchange.github.io/disruptor/)。採用無中介者系統，開發者可建立輕量、快速且高效的分散式系統組件通訊管道。

5. **資料庫作為佇列：** 某些情況下，傳統關聯式或 NoSQL 資料庫可用作訊息佇列，使與現有系統整合更簡單，並利用熟悉的資料管理工具。此方法特別適用於較小規模應用，或從單體架構遷移至分散式架構的過渡階段。使用資料庫作為佇列，可善用內建的交易、索引與查詢功能有效管理訊息。範例如 PostgreSQL 的 LISTEN/NOTIFY 功能或利用 Amazon DynamoDB Streams。雖然資料庫作為佇列的效能與擴展性及功能通常不及專用訊息服務，有時被視為反模式，但在特定使用場景或需求較低時是可行選項。

### 比較表

|                        | 效能               | 擴展性             | 彈性                 | 複雜度             | 功能性               | 易用性               |
|------------------------|--------------------|--------------------|----------------------|---------------------|----------------------|----------------------|
| 通用訊息佇列           | 中等               | 中等               | 高                   | 中等                | 高                   | 中等                 |
| 發布/訂閱              | 中等至高           | 高                 | 高                   | 中等                | 中等至高             | 中等至高             |
| 串流處理               | 高                 | 高                 | 中等至高             | 高                  | 高                   | 中等                 |
| 無中介者               | 高                 | 中等               | 中等                 | 低至中等            | 中等                 | 高                   |
| 資料庫作為佇列         | 低至中等           | 低至中等           | 中等                 | 低                  | 低至中等             | 高                   |

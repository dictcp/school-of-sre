
# 先決條件

建議具備網路安全、TCP 以及數據中心設置的基本知識，並熟悉相關常用術語。此外，讀者應該先閱讀 School of SRE 的內容 - 

- [Linux 網路](http://linkedin.github.io/school-of-sre/level101/linux_networking/intro/)

- [系統設計](http://linkedin.github.io/school-of-sre/level101/systems_design/intro/)

- [安全性](http://linkedin.github.io/school-of-sre/level101/security/intro/)

# 課程預期內容

本部分將涵蓋數據中心基礎架構如何根據不同應用需求劃分，以及決定應用程式部署位置時的考量。這些主要基於以下幾點：安全性、規模、RTT（延遲）、基礎設施特性。

這些主題將詳細介紹：

安全性 - 將探討面對外部/內部用戶時，服務可能遇到的威脅向量，以及部署時可考慮的潛在緩解方案。會涉及周邊安全、防止 [DDoS](https://zh.wikipedia.org/wiki/%E6%B6%88%E6%AD%A2%E6%9C%8D%E5%8B%99%E6%94%BB%E5%87%BB) 攻擊、網路界限設置及伺服器叢集的環繞防護。

規模 - 部署大規模應用需深入了解基礎架構的資源可用性、故障域、擴展選項，如使用 Anycast、第4層/第7層負載平衡器、基於 DNS 的負載平衡。

RTT（延遲） - 延遲在判斷分散式服務/應用整體效能中扮演關鍵角色，因為用戶請求會在主機間發生多次呼叫。

基礎設施特性 - 需考量的部分有：底層數據中心基礎設施是否支援 ToR 韌性（如連結聚合（bonds）、BGP（邊界閘道協定）、Anycast 服務支援、負載平衡、防火牆、服務品質QoS等功能。

# 本課程未涵蓋內容

雖然這些參數對應用設計有一定影響，我們將不深入探討設計細節。這些主題範圍廣泛，課程目標是介紹各參數術語及其相關性，而非提供每項技術的完整詳解。

# 課程內容
1. [安全性](http://linkedin.github.io/school-of-sre/level102/networking/security/)
2. [規模](https://linkedin.github.io/school-of-sre/level102/networking/scale/)
3. [RTT](http://linkedin.github.io/school-of-sre/level102/networking/rtt/)
4. [基礎設施特性](http://linkedin.github.io/school-of-sre/level102/networking/Infrastructure-features/)
5. [總結](http://linkedin.github.io/school-of-sre/level102/networking/Conclusion/)


# 術語說明

在討論各主題前，先了解幾個常用術語：

雲端

指由不同供應商提供的託管解決方案，如 Azure、AWS、GCP。企業可將應用部署於此，供公共或私人使用。

本地架設（On-prem）

指企業自行建置與管理的實體數據中心（DC）基礎設施，可用於私人網路存取，或公共連線（如透過網際網路的用戶）。

葉節點交換機（Leaf switch/ToR）

指數據中心中伺服器所連接的交換機。此類交換機有多種稱呼，如存取交換機、機架頂交換機（Top of the Rack，ToR）、葉節點交換機。

「葉節點交換機」一詞來自 [Spine-leaf 架構](https://searchdatacenter.techtarget.com/definition/Leaf-spine)，其中存取交換機稱為葉節點交換機。Spine-leaf 架構常見於大型/超大規模數據中心，具高度擴展性，且在建置和實施交換機時效率更高。有時也稱為 Clos 架構。

脊椎交換機（Spine switch）

為多個葉節點交換機的聚合點，負責葉節點間的通訊，並連接到數據中心的上層基礎設施。

數據中心網路架構（DC fabric）

隨著數據中心擴大，需將多個 Clos 網路互連以支持規模，而 Fabric 交換機協助完成此互連。

機櫃

指裝置伺服器和 ToR 交換機的機架，一機櫃即指整個機架。

BGP

代表邊界閘道協定（Border Gateway Protocol），用以在路由器和交換機間交換路由資訊。BGP 為互聯網及數據中心常用的通訊協定。此外，亦有其他協定如 OSPF 使用於替代。

VPN

虛擬私人網路，為透過公眾網路（網際網路）將兩個私有網路（如辦公室、數據中心）互連的隧道解決方案。VPN 隧道在資料傳送前會加密，以提升安全性。

網路介面卡（NIC）

指伺服器中的模組，包含乙太網路連接埠及系統匯流排介面，用來連接交換機（通常是 ToR）。

流量（Flow）

指兩個節點（伺服器、交換機、路由器等）間的資料交換，具有共通的參數，如來源/目的 IP 位址、來源/目的連接埠號碼、IP 協定號碼。流量有助於追蹤特定節點間的交換會話（如檔案傳輸、HTTP 連線等）。

等價多路徑（ECMP）

指交換機或路由器能將流向同一目的地的流量分配至多條出口介面。流量資訊用以建構 hash 值，依此選擇出口介面。流量一旦分配到特定介面，該流的所有封包僅經該介面輸出，避免封包順序錯亂。

往返時間（RTT）

衡量封包從來源送達目的地並返回所需的時間，常用於網路效能測試與故障排除。

TCP 吞吐量

指兩節點間達成的資料傳輸速率，受 RTT、封包大小、視窗大小等多種參數影響。

單播（Unicast）

指單一來源至單一目的地的流量，如 ssh 會話，一對一通訊。

任播（Anycast）

類似單播，但目的端為多個節點之一。單一來源可將流量傳送到該群組中任一目的主機。透過在多台伺服器配置相同 IP 地址實現，每個新流量會被分配至其中一台伺服器。

多播（Multicast）

指單一來源向多個目的地傳送流量。為實現此需求，網路路由器會複製流量至不同主機（這些主機需註冊成為特定多播群組成員）。

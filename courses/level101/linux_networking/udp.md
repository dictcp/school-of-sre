# UDP

UDP 是一種傳輸層協議。DNS 是一種應用層協議，且大多數時間是運行於 UDP 之上。在深入了解 UDP 之前，讓我們先試著理解什麼是應用層和傳輸層。DNS 協議由 DNS 用戶端（例如 `dig`）和 DNS 伺服器（例如 `named`）使用。傳輸層確保 DNS 請求能到達 DNS 伺服器的程序，同樣的回應也能送回 DNS 用戶端的程序。一台系統上可以執行多個程序，並且它們可以監聽任何[埠口](https://en.wikipedia.org/wiki/Port_(computer_networking))。DNS 伺服器通常監聽的埠號是 `53`。當用戶端發出 DNS 請求後，填寫好必要的應用程式負載，它會透過 **sendto** 系統呼叫將負載傳給核心。核心挑選一個隨機的埠號（[>1024](https://www.cyberciti.biz/tips/linux-increase-outgoing-network-sockets-range.html)）作為來源埠，將 53 設為目的埠，並將封包送到低層網路。當伺服器端的核心收到封包時，它會檢查埠號並將封包排入 DNS 伺服器程序的應用緩衝區，此程序會呼叫 **recvfrom** 來讀取封包。這個過程在核心內部稱為多工（將多個應用程序的封包合併至同一低層）與解多工（將單一低層的封包分配給多個應用）。多工與解多工是由傳輸層執行的。

UDP 是最簡單的傳輸層協議之一，只做多工與解多工。另一種常見的傳輸層協議 TCP 則做更多事情，例如可靠傳輸、流量控制和擁塞控制。UDP 設計成輕量且通訊開銷低，因此不做除了多工與解多工以外的任何事。如果在 UDP 上運行的應用程式需要 TCP 的功能，他們必須自己實作。

此[Python Wiki 範例](https://wiki.python.org/moin/UdpCommunication)展示了一個 UDP 用戶端與伺服器的範例，當中「Hello World」為應用負載，傳送到監聽埠號為 `5005` 的伺服器。伺服器接收該封包並印出來自客戶端的「Hello World」字串。

## SRE 角色的應用

1. 若底層網路速度緩慢，且 UDP 層無法將封包排入至網路層，應用程式的 `sendto` 系統呼叫會被阻塞，直到核心釋放出部分緩衝區空間。此情況會影響系統的吞吐量。調高使用 [sysctl 變數](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/tuning_and_optimizing_red_hat_enterprise_linux_for_oracle_9i_and_10g_databases/sect-oracle_9i_and_10g_tuning_guide-adjusting_network_settings-changing_network_kernel_settings) *net.core.wmem_max* 與 *net.core.wmem_default* 的寫入記憶體緩衝值，可以給應用程式在面對慢速網路時更好的緩衝空間。
2. 同樣地，若接收端程式消費緩慢，核心無法將更多封包排入已滿的緩衝區，將會丟棄這些封包。因為 UDP 不保證可靠性，這些丟包會造成資料遺失，除非由應用層追蹤。調高 *rmem_default* 與 *rmem_max* sysctl 變數，可以讓慢速的應用程式在面對快速發送端時有緩衝的空間。

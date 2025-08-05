# Linux 網路基礎

## 先備知識

- 對 TCP/IP 協議堆疊中常用術語如 DNS、TCP、UDP 和 HTTP 有高階認識
- [Linux 指令行基礎](https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/)

## 本課程的學習期待

在整個課程中，我們會講解站點可靠性工程師（SRE）如何優化系統以提升其網路堆疊的效能，以及當網路堆疊中任一層出現問題時如何進行故障排除。本課程將深入探討傳統 TCP/IP 協議堆疊的每一層，期望 SRE 能夠對網際網路的運作有超越概觀的清晰認知。

## 本課程不涵蓋的內容

本課程著重在基礎知識。我們不會涵蓋如 [HTTP/2.0](https://en.wikipedia.org/wiki/HTTP/2)、[QUIC](https://en.wikipedia.org/wiki/QUIC)、[TCP 擁塞控制協議](https://en.wikipedia.org/wiki/TCP_congestion_control)、[Anycast](https://en.wikipedia.org/wiki/Anycast)、[BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)、[CDN](https://en.wikipedia.org/wiki/Content_delivery_network)、[通道技術](https://en.wikipedia.org/wiki/Virtual_private_network) 及 [多播](https://en.wikipedia.org/wiki/Multicast) 等概念。我們期望本課程能提供學習這些概念所需的相關基礎。

## 課程概覽

本課程探討問題：「當你在瀏覽器中開啟 [linkedin.com](https://www.linkedin.com) 時，會發生什麼事？」課程按照 TCP/IP 協定堆疊的流程進行講解。更具體地，本課程涵蓋應用層協議（DNS 和 HTTP）、運輸層協議（UDP 和 TCP）、網路層協議（IP）以及資料鏈路層協議等主題。

## 課程內容

1. [DNS](https://linkedin.github.io/school-of-sre/level101/linux_networking/dns/)
2. [UDP](https://linkedin.github.io/school-of-sre/level101/linux_networking/udp/)
3. [HTTP](https://linkedin.github.io/school-of-sre/level101/linux_networking/http/)
4. [TCP](https://linkedin.github.io/school-of-sre/level101/linux_networking/tcp/)
5. [IP 路由](https://linkedin.github.io/school-of-sre/level101/linux_networking/ipr/)

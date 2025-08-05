# DNS

網域名稱是網站的人類易讀名稱。網際網路只認識 IP 位址，但由於記憶一串毫無意義的數字不實際，因此改用網域名稱。這些網域名稱會由 DNS 基礎架構轉換成 IP 位址。當有人嘗試在瀏覽器中打開 [www.linkedin.com](https://www.linkedin.com) 時，瀏覽器會嘗試將 [www.linkedin.com](https://www.linkedin.com) 轉換成 IP 位址。此過程稱為 DNS 解析。下面用簡單的偽代碼描述這個過程：

```python
ip, err = getIPAddress(domainName)
if err:
  print("unknown Host Exception while trying to resolve:%s".format(domainName))
```

現在，讓我們了解 `getIPAddress` 函數內部會做什麼。瀏覽器會有自己的 DNS 快取，會先檢查是否已有對應的 `domainName` 到 IP 位址的映射，若有，瀏覽器便直接使用該 IP 位址。若無此映射，瀏覽器會呼叫 `gethostbyname` 系統呼叫，請作業系統找出該 `domainName` 對應的 IP 位址。

```python
def getIPAddress(domainName):
    resp, fail = lookupCache(domainName)
    If not fail:
       return resp
    else:
       resp, err = gethostbyname(domainName)
       if err:
         return null, err
       else:
          return resp
```

接著，我們理解當呼叫 [gethostbyname](https://man7.org/linux/man-pages/man3/gethostbyname.3.html) 函數時，作業系統核心會做什麼。Linux 作業系統會查看檔案 [/etc/nsswitch.conf](https://man7.org/linux/man-pages/man5/nsswitch.conf.5.html)，通常會有一行：

```bash
hosts:      files dns
```

此行表示作業系統先查找檔案 (`/etc/hosts`)，若無對應，則使用 DNS 協定進行解析。

`/etc/hosts` 檔案格式如下：

IP 位址 完整網域名稱 [完整網域名稱].*

```bash
127.0.0.1 localhost.localdomain localhost
::1 localhost.localdomain localhost
```

若該檔案中存在對應此網域的映射，作業系統即回傳該 IP 位址。讓我們在此檔案新增一行：

```bash
127.0.0.1 test.linkedin.com
```

然後執行 ping 命令去測試 [test.linkedin.com](https://test.linkedin.com/)。

```bash
ping test.linkedin.com -n
```

```bash
PING test.linkedin.com (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.047 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.036 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.037 ms
```

如前所述，若 `/etc/hosts` 中沒有對應，作業系統會透過 DNS 協定嘗試解析。Linux 系統會向 `/etc/resolv.conf` 中的第一個 IP 發送 DNS 請求。若無回應，會依序向 `resolv.conf` 中其他伺服器請求。這些伺服器稱為 DNS 解析器。DNS 解析器通常由 [DHCP](https://zh.wikipedia.org/wiki/DHCP) 或系統管理員靜態設定配置。
[Dig](https://linux.die.net/man/1/dig) 是一個用戶空間的 DNS 工具，可建立並發送 DNS 請求給解析器，並將回應輸出在終端機。

```bash
# 在一個 shell 中執行此指令以捕捉所有 DNS 請求
sudo tcpdump -s 0 -A -i any port 53
# 在另一個 shell 中發送 dig 請求
dig linkedin.com
```

```bash
13:19:54.432507 IP 172.19.209.122.56497 > 172.23.195.101.53: 527+ [1au] A? linkedin.com. (41)
....E..E....@.n....z...e...5.1.:... .........linkedin.com.......)........
13:19:54.485131 IP 172.23.195.101.53 > 172.19.209.122.56497: 527 1/0/1 A 108.174.10.10 (57)
....E..U..@.|.	....e...z.5...A...............linkedin.com..............3..l.

..)........
```

封包截取顯示向 `172.23.195.101:53`（即 `/etc/resolv.conf` 中的解析器）送出 [linkedin.com](https://www.linkedin.com/) 的 DNS 請求，並由 `172.23.195.101` 回傳 [linkedin.com](https://www.linkedin.com/) 的 IP 位址 `108.174.10.10`。

現在，我們來了解 DNS 解析器如何尋找 [linkedin.com](https://www.linkedin.com/) 的 IP 位址。DNS 解析器首先查看快取。由於網路中許多裝置都可能查詢此網域名稱，因此解析結果可能已有快取。若快取失敗，開始 DNS 解析流程。DNS 伺服器會將 “linkedin.com” 拆解成 “.”、 “com.” 與 “linkedin.com.”，並從 “.” 開始解析。“.” 是稱為根網域（Root Domain），其 IP 是解析器已知的。DNS 解析器詢問根域名伺服器以尋找對「com.」負責的頂層域（TLD）伺服器。取得 “com.” 的 TLD 域名伺服器位址後，再查詢該 TLD 域名伺服器以獲得 “linkedin.com” 的權威域名伺服器。最後，解析器再查詢 LinkedIn 的權威域名伺服器以取得 “linkedin.com” 的 IP 位址。整個流程可透過執行以下指令觀察：

```bash
dig +trace linkedin.com
```

```bash
linkedin.com.		3600	IN	A	108.174.10.10
```

此 DNS 回應有五個欄位，第一欄是查詢對象，最後一欄是回應結果。第二欄是存活時間（TTL，Time-to-Live），表示 DNS 回應的有效秒數。此例中字串對應的 IP 有效時間為 1 小時。這就是為什麼解析器與應用程式（瀏覽器）會保留快取的方式。超過 1 小時後，再次請求 [linkedin.com](https://www.linkedin.com/) 將被視為快取失效，必須重新進行整個解析流程。
第四欄表示 DNS 回應/請求的類型。常見的 DNS 查詢類型包括
A, AAAA, NS, TXT, PTR, MX 與 CNAME。

- A 紀錄回傳網域的 IPv4 位址
- AAAA 紀錄回傳網域的 IPv6 位址
- NS 紀錄回傳該網域的權威名稱伺服器
- CNAME 紀錄為網域別名。有些網域指向其他網域，解譯該別名網域會回傳 IP，而這 IP 也適用於原先的網域。例如 [www.linkedin.com](https://www.linkedin.com) 的 IP 就與 `2-01-2c3e-005a.cdx.cedexis.net` 相同。
- 為簡潔起見，本篇不深入討論其他 DNS 紀錄類型，詳細 RFC 可參考[此處](https://zh.wikipedia.org/wiki/DNS_記錄類型列表)。

```bash
dig A linkedin.com +short
108.174.10.10

dig AAAA linkedin.com +short
2620:109:c002::6cae:a0a

dig NS linkedin.com +short
dns3.p09.nsone.net.
dns4.p09.nsone.net.
dns2.p09.nsone.net.
ns4.p43.dynect.net.
ns1.p43.dynect.net.
ns2.p43.dynect.net.
ns3.p43.dynect.net.
dns1.p09.nsone.net.

dig www.linkedin.com CNAME +short
2-01-2c3e-005a.cdx.cedexis.net.
```

有了 DNS 的基礎知識，我們來看看 SRE（Site Reliability Engineer）角色中 DNS 的應用場景。

## SRE 角色中的應用

本節涵蓋 SRE 能從 DNS 導出的一些常見解決方案。

1. 每家公司都必須維護內部 DNS 基礎架構，支援企業內網網站與內部服務（資料庫、內部應用如 Wiki 等）。基礎架構團隊需維護這套 DNS 系統。此基礎架構必須優化並具備可擴展性，避免成為單點故障來源。內部 DNS 的故障可能導致微服務 API 呼叫失敗，進而引起級聯效應。
2. DNS 可用於服務發現，例如主機名稱 `serviceb.internal.example.com` 可列舉在 `example.com` 企業內部執行服務 `b` 的實例。雲端供應商提供啟用 DNS 服務發現的選項（[範例](https://docs.aws.amazon.com/whitepapers/latest/microservices-on-aws/service-discovery.html#dns-based-service-discovery)）。
3. DNS 被雲端及 CDN 供應商用於服務擴展。在 Azure/AWS 中，負載平衡器會被指派一個 CNAME 取代直接 IP 位址。當服務擴展時，系統會調整這些 CNAME 指向的真實 IP 位址，這也是此類別名域的 A 紀錄生存時間常維持在約 1 分鐘的原因之一。
4. DNS 也能讓客戶端取得距離其位置較近的 IP 位址，使其 HTTP 呼叫回應速度更快，尤其是公司於不同地理區域皆有部署。
5. SRE 需理解 DNS 回應缺乏驗證機制，容易遭冒充。此安全問題由其它協定如 HTTPS 解決（後續會提到），而 DNSSEC 可防止偽造或遭竄改的 DNS 回應。
6. 過期的 DNS 快取可能造成問題。有些[應用](https://stackoverflow.com/questions/1256556/how-to-make-java-honor-the-dns-caching-timeout) 可能仍使用已過期的 DNS 紀錄進行 API 呼叫，維運時需特別留意這點。
7. DNS 負載平衡與服務發現必須瞭解 TTL，因此更改 DNS 紀錄後，伺服器只能在該 TTL 期限過後才從流量池中刪除。若未依此執行，部分流量可能會失敗，因為伺服器尚被快取期望有效。

以上便是 DNS 基礎與 SRE 相關應用的介紹。

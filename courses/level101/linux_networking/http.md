# HTTP

到目前為止，我們只取得了 [linkedin.com](https://www.linkedin.com/) 的 IP 位址。[linkedin.com](https://www.linkedin.com/) 的 HTML 頁面是由瀏覽器透過 HTTP 協定來提供並渲染的。瀏覽器向前面決定的伺服器 IP 發送 HTTP 請求。請求中包含動詞 GET、PUT、POST，後面接路徑與查詢參數，以及多行的鍵-值對，用來提供關於用戶端的資訊和其能力，例如可以接受的內容型態，以及主體（通常出現在 POST 或 PUT 中）。

```bash
# 例如，在你的容器中執行下列命令，觀察標頭內容
curl linkedin.com -v
```
```bash
* Connected to linkedin.com (108.174.10.10) port 80 (#0)
> GET / HTTP/1.1
> Host: linkedin.com
> User-Agent: curl/7.64.1
> Accept: */*
> 
< HTTP/1.1 301 Moved Permanently
< Date: Mon, 09 Nov 2020 10:39:43 GMT
< X-Li-Pop: prod-esv5
< X-LI-Proto: http/1.1
< Location: https://www.linkedin.com/
< Content-Length: 0
< 
* Connection #0 to host linkedin.com left intact
* Closing connection 0
```

在這裡，第一行中 `GET` 是動詞，`/` 是路徑，`1.1` 是 HTTP 協定版本。接著是多組鍵值對，提供客戶端能力與一些伺服器資訊。伺服器回應包含 HTTP 版本、[狀態碼與狀態訊息](https://zh.wikipedia.org/wiki/HTTP%E7%8B%80%E6%85%8B%E7%A2%BC%E6%88%96%E8%80%85%E7%8B%80%E6%85%8B%E6%B6%88%E6%81%AF)。狀態碼中 `2xx` 代表成功，`3xx` 代表重導向，`4xx` 表客戶端錯誤，`5xx` 表伺服器端錯誤。

我們接著來看看 HTTP/1.0 與 HTTP/1.1 的差異。

```bash
# 在終端機輸入
telnet  www.linkedin.com 80
# 複製並貼上以下內容，並在最後空一行，在 telnet 的標準輸入中完成
GET / HTTP/1.1
HOST:linkedin.com
USER-AGENT: curl

```

這樣會取得伺服器回應，並等待後續輸入，因為到底層與 [www.linkedin.com](https://www.linkedin.com/) 的連線可以被重複使用以執行更多請求。透過 TCP 的知識可以理解這種好處。但在 HTTP/1.0 中，伺服器回應後會立即關閉連線，也就是說每個請求都需要新開連線。HTTP/1.1 中，一條開啟的連線中只能有一個「執行中（inflight）」的請求，但可以連續不斷用同一條連線做多次請求。HTTP/2.0 相較 HTTP/1.1 的優點是，可同時有多個執行中請求共用同一連線。我們暫且只討論通用 HTTP，而不深入各版本協定細節，這些都能在課程結束後掌握。

HTTP 稱作**無狀態協定 (stateless protocol)**。接下來我們理解什麼是「無狀態」。假設我們已登入 [linkedin.com](https://www.linkedin.com/)，客戶端每個向 [linkedin.com](https://www.linkedin.com/) 發出的請求並不記憶用戶上下文，若每次瀏覽網頁時都要求使用者登入，將毫無意義。HTTP 的這個問題由 *COOKIE* 解決。當使用者登入後，會建立一個 session，並以 *SET-COOKIE* 標頭把 session 識別碼傳給瀏覽器。瀏覽器會保留該 cookie 直到伺服器設定的過期時間，之後每次向 [linkedin.com](https://www.linkedin.com/) 發送請求都會攜帶 cookie。更多關於 Cookie 的資訊可查看 [這裡](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Cookies)。Cookie 是像密碼一樣重要的資訊，且由於 HTTP 是純文字協定，中間人攻擊者可以攔截密碼或 cookie，進而侵犯用戶隱私。類似地，如 DNS 節所提，若 [linkedin.com](https://www.linkedin.com/) 的 IP 被惡意偽裝，會對用戶造成釣魚攻擊，令用戶誤將 LinkedIn 密碼輸入至惡意網站。為了解決這些問題，HTTPS 被引入，且其使用必須被強制執行。

HTTPS 需要提供伺服器身份識別以及用戶端與伺服器間資料的加密。伺服器管理員需產生一對私鑰與公鑰，以及憑證請求，並由憑證授權中心 (CA) 簽署該請求並轉成憑證。伺服器管理員需將憑證與私鑰設定到網頁伺服器上。憑證中載有伺服器資訊（例如服務的網域名稱、到期日），以及伺服器的公鑰。私鑰是伺服器的秘密，若私鑰外洩，該伺服器的信任將化為烏有。連線時，客戶端先送出 HELLO，伺服器回傳憑證給客戶端。客戶端透過檢查憑證的有效期限、是否由信任的機構簽發，以及憑證的主機名稱是否與伺服器相符等步驟，驗證該憑證以確保伺服器正確、無釣魚風險。驗證通過後，客戶端使用伺服器的公鑰加密，與伺服器協商共用的對稱金鑰與密碼演算法。除了擁有私鑰的伺服器外，任何人無法讀懂這些資料。協商完成後，雙方使用此對稱金鑰與演算法加密往後的資料傳輸，包括首次的 HTTP 請求。之所以從非對稱加密轉為對稱加密，是因為對稱加密通常在資源使用上較輕量，避免對用戶端設備造成過大負擔。

```bash
# 在終端機執行以下指令，查看憑證相關詳細資訊，如主旨名稱(網域名稱)、發行者資訊、到期日
curl https://www.linkedin.com -v 
```
```bash
* Connected to www.linkedin.com (13.107.42.14) port 443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/cert.pem
  CApath: none
* TLSv1.2 (OUT), TLS handshake, Client hello (1):
} [230 bytes data]
* TLSv1.2 (IN), TLS handshake, Server hello (2):
{ [90 bytes data]
* TLSv1.2 (IN), TLS handshake, Certificate (11):
{ [3171 bytes data]
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
{ [365 bytes data]
* TLSv1.2 (IN), TLS handshake, Server finished (14):
{ [4 bytes data]
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
} [102 bytes data]
* TLSv1.2 (OUT), TLS change cipher, Change cipher spec (1):
} [1 bytes data]
* TLSv1.2 (OUT), TLS handshake, Finished (20):
} [16 bytes data]
* TLSv1.2 (IN), TLS change cipher, Change cipher spec (1):
{ [1 bytes data]
* TLSv1.2 (IN), TLS handshake, Finished (20):
{ [16 bytes data]
* SSL connection using TLSv1.2 / ECDHE-RSA-AES256-GCM-SHA384
* ALPN, server accepted to use h2
* Server certificate:
*  subject: C=US; ST=California; L=Sunnyvale; O=LinkedIn Corporation; CN=www.linkedin.com
*  start date: Oct  2 00:00:00 2020 GMT
*  expire date: Apr  2 12:00:00 2021 GMT
*  subjectAltName: host "www.linkedin.com" matched cert's "www.linkedin.com"
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert SHA2 Secure Server CA
*  SSL certificate verify ok.
* Using HTTP2, server supports multi-use
* Connection state changed (HTTP/2 confirmed)
* Copying HTTP/2 data in stream buffer to connection buffer after upgrade: len=0
* Using Stream ID: 1 (easy handle 0x7fb055808200)
* Connection state changed (MAX_CONCURRENT_STREAMS == 100)!
  0 82117    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
* Connection #0 to host www.linkedin.com left intact
HTTP/2 200 
cache-control: no-cache, no-store
pragma: no-cache
content-length: 82117
content-type: text/html; charset=utf-8
expires: Thu, 01 Jan 1970 00:00:00 GMT
set-cookie: JSESSIONID=ajax:2747059799136291014; SameSite=None; Path=/; Domain=.www.linkedin.com; Secure
set-cookie: lang=v=2&lang=en-us; SameSite=None; Path=/; Domain=linkedin.com; Secure
set-cookie: bcookie="v=2&70bd59e3-5a51-406c-8e0d-dd70befa8890"; domain=.linkedin.com; Path=/; Secure; Expires=Wed, 09-Nov-2022 22:27:42 GMT; SameSite=None
set-cookie: bscookie="v=1&202011091050107ae9b7ac-fe97-40fc-830d-d7a9ccf80659AQGib5iXwarbY8CCBP94Q39THkgUlx6J"; domain=.www.linkedin.com; Path=/; Secure; Expires=Wed, 09-Nov-2022 22:27:42 GMT; HttpOnly; SameSite=None
set-cookie: lissc=1; domain=.linkedin.com; Path=/; Secure; Expires=Tue, 09-Nov-2021 10:50:10 GMT; SameSite=None
set-cookie: lidc="b=VGST04:s=V:r=V:g=2201:u=1:i=1604919010:t=1605005410:v=1:sig=AQHe-KzU8i_5Iy6MwnFEsgRct3c9Lh5R"; Expires=Tue, 10 Nov 2020 10:50:10 GMT; domain=.linkedin.com; Path=/; SameSite=None; Secure
x-fs-txn-id: 2b8d5409ba70
x-fs-uuid: 61bbf94956d14516302567fc882b0000
expect-ct: max-age=86400, report-uri="https://www.linkedin.com/platform-telemetry/ct"
x-xss-protection: 1; mode=block
content-security-policy-report-only: default-src 'none'; connect-src 'self' www.linkedin.com www.google-analytics.com https://dpm.demdex.net/id lnkd.demdex.net blob: https://linkedin.sc.omtrdc.net/b/ss/ static.licdn.com static-exp1.licdn.com static-exp2.licdn.com static-exp3.licdn.com; script-src 'sha256-THuVhwbXPeTR0HszASqMOnIyxqEgvGyBwSPBKBF/iMc=' 'sha256-PyCXNcEkzRWqbiNr087fizmiBBrq9O6GGD8eV3P09Ik=' 'sha256-2SQ55Erm3CPCb+k03EpNxU9bdV3XL9TnVTriDs7INZ4=' 'sha256-S/KSPe186K/1B0JEjbIXcCdpB97krdzX05S+dHnQjUs=' platform.linkedin.com platform-akam.linkedin.com platform-ecst.linkedin.com platform-azur.linkedin.com static.licdn.com static-exp1.licdn.com static-exp2.licdn.com static-exp3.licdn.com; img-src data: blob: *; font-src data: *; style-src 'self' 'unsafe-inline' static.licdn.com static-exp1.licdn.com static-exp2.licdn.com static-exp3.licdn.com; media-src dms.licdn.com; child-src blob: *; frame-src 'self' lnkd.demdex.net linkedin.cdn.qualaroo.com; manifest-src 'self'; report-uri https://www.linkedin.com/platform-telemetry/csp?f=g
content-security-policy: default-src *; connect-src 'self' https://media-src.linkedin.com/media/ www.linkedin.com s.c.lnkd.licdn.com m.c.lnkd.licdn.com s.c.exp1.licdn.com s.c.exp2.licdn.com m.c.exp1.licdn.com m.c.exp2.licdn.com wss://*.linkedin.com dms.licdn.com https://dpm.demdex.net/id lnkd.demdex.net blob: https://accounts.google.com/gsi/status https://linkedin.sc.omtrdc.net/b/ss/ www.google-analytics.com static.licdn.com static-exp1.licdn.com static-exp2.licdn.com static-exp3.licdn.com media.licdn.com media-exp1.licdn.com media-exp2.licdn.com media-exp3.licdn.com; img-src data: blob: *; font-src data: *; style-src 'unsafe-inline' 'self' static-src.linkedin.com *.licdn.com; script-src 'report-sample' 'unsafe-inline' 'unsafe-eval' 'self' spdy.linkedin.com static-src.linkedin.com *.ads.linkedin.com *.licdn.com static.chartbeat.com www.google-analytics.com ssl.google-analytics.com bcvipva02.rightnowtech.com www.bizographics.com sjs.bizographics.com js.bizographics.com d.la4-c1-was.salesforceliveagent.com slideshare.www.linkedin.com https://snap.licdn.com/li.lms-analytics/ platform.linkedin.com platform-akam.linkedin.com platform-ecst.linkedin.com platform-azur.linkedin.com; object-src 'none'; media-src blob: *; child-src blob: lnkd-communities: voyager: *; frame-ancestors 'self'; report-uri https://www.linkedin.com/platform-telemetry/csp?f=l
x-frame-options: sameorigin
x-content-type-options: nosniff
strict-transport-security: max-age=2592000
x-li-fabric: prod-lva1
x-li-pop: afd-prod-lva1
x-li-proto: http/2
x-li-uuid: Ybv5SVbRRRYwJWf8iCsAAA==
x-msedge-ref: Ref A: CFB9AC1D2B0645DDB161CEE4A4909AEF Ref B: BOM02EDGE0712 Ref C: 2020-11-09T10:50:10Z
date: Mon, 09 Nov 2020 10:50:10 GMT

* Closing connection 0
```

在這裡，我的系統信任的憑證授權中心清單存放於 `/etc/ssl/cert.pem`。cURL 會檢查憑證中的 CN（主旨名稱）部分是否為 [www.linkedin.com](https://www.linkedin.com/)，同時確認憑證未過期，並使用簽發者 DigiCert 的公鑰（同樣存在 `/etc/ssl/cert.pem`）驗證憑證簽章。驗證通過後，使用 [www.linkedin.com](https://www.linkedin.com/) 的公鑰協商密碼套件 `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384` 與對稱金鑰。隨後包含首次 HTTP 請求的資料傳輸皆會使用該密碼套件及對稱金鑰進行加密。

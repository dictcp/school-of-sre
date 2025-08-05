# Python、網頁與 Flask

早期網站都非常簡單。它們是簡單的靜態 HTML 內容。網路伺服器會在指定埠口監聽，根據收到的 HTTP 請求，從磁碟讀取檔案並回傳作為回應。但隨著時間演進，網站變得越來越複雜且動態化。依據請求內容，系統需要執行多重操作，例如讀取資料庫或呼叫其他 API，最後回傳某種回應（HTML 頁面、JSON 內容等）。

因為處理 Web 請求已不再是簡單的讀檔案並回傳內容，我們必須針對每個 HTTP 請求進行程式處理，執行各種操作後組成回應。

## Socket

雖然我們有像 Flask 這樣的框架，HTTP 協定本身仍是在 TCP 協定之上運作的協定。因此，我們先建立一台 TCP 伺服器，傳送 HTTP 請求並檢視請求的內容。請注意這不是 Socket 程式設計的教學，我們這裡做的是從最底層檢視 HTTP 協定，看看它的內容長什麼樣子。（參考：[RealPython 的 Python Socket 程式設計指南](https://realpython.com/python-sockets/)）

```python
import socket

HOST = '127.0.0.1'  # 標準迴圈接口 (localhost)
PORT = 65432        # 監聽的非特權埠口（大於1023）

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
   s.bind((HOST, PORT))
   s.listen()
   conn, addr = s.accept()
   with conn:
       print('Connected by', addr)
       while True:
           data = conn.recv(1024)
           if not data:
               break
           print(data)
```

接著，我們在瀏覽器打開 `localhost:65432` ，輸出會是：

```bash
Connected by ('127.0.0.1', 54719)
b'GET / HTTP/1.1\r\nHost: localhost:65432\r\nConnection: keep-alive\r\nDNT: 1\r\nUpgrade-Insecure-Requests: 1\r\nUser-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.83 Safari/537.36 Edg/85.0.564.44\r\nAccept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9\r\nSec-Fetch-Site: none\r\nSec-Fetch-Mode: navigate\r\nSec-Fetch-User: ?1\r\nSec-Fetch-Dest: document\r\nAccept-Encoding: gzip, deflate, br\r\nAccept-Language: en-US,en;q=0.9\r\n\r\n'
```

細看內容會像是 HTTP 協定的格式，也就是：

```
HTTP 方法 URI 路徑 HTTP 版本
標頭行 用分隔符分隔
```

所以雖然它是一串位元組的資料，但只要了解 [HTTP 協定規範](https://tools.ietf.org/html/rfc2616)，你可以將字串拆分（例如用 `\r\n` 拆分）並解析出有意義的資訊。

## Flask

Flask 和其他類似框架所做的事情基本上就是上一節討論的，只是更為複雜精緻。他們在 TCP Socket 某個埠口監聽、接收 HTTP 請求，根據協定格式解析資料，並以方便使用的方式提供給開發者。

也就是說，你可以用 `request.headers` 訪問請求標頭，Flask 會將剛剛上述的字串（用 `\r\n` 分割）解析並轉成這樣方便的結構。

另一個範例是：在 Flask 中用 `@app.route("/hello")` 註冊路由。Flask 會在內部維護一個映射表，將 `/hello` 映射到你所裝飾的函式。當請求的路徑是 `/hello`（第一行以空格拆分的第二個部份）時，Flask 就會呼叫該函式並送回函式的執行結果。

其他程式語言的各種 Web 框架也都相似。基本上他們的工作就是了解 HTTP 協定，解析 HTTP 請求資料，並提供一個方便的介面讓程式設計師操作 HTTP 請求。

看起來並沒有什麼魔法吧？

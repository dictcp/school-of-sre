# URL 縮短應用程式

讓我們用 Flask 建立一個非常簡單的 URL 縮短應用程式，並嘗試涵蓋開發過程中的所有面向，包括可靠性方面。我們不會建立使用者介面，將設計一組最簡單的 API，使該應用程式能良好運作。

## 設計

我們不會直接跳去撰寫程式碼。首先要做的是蒐集需求，提出一個方案，請同儕審查設計/方案，持續演化、迭代並記錄決策與取捨，最後才實作。雖然這裡不會寫出完整的設計文件，但會在此提出一些重要的設計問題。

### 1. 高階操作與 API 端點

由於是 URL 縮短應用程式，我們需要一個 API 能在輸入原始連結後產生縮短連結，還有一個 API/端點能接受縮短的連結並重新導向至原始 URL。我們不會包含使用者相關的功能，以保持簡潔。這兩個 API 應足以讓應用程式可用且有效。

### 2. 如何縮短？

給定一個 URL，我們需要產生它的縮短版本。一種方法是對每個連結使用隨機字元。另一種方法是使用某種雜湊演算法。這樣做的好處是相同的連結會重複使用相同的雜湊值。比如：若很多人都縮短 `https://www.linkedin.com`，他們將會得到相同的縮短值，比起若採用隨機字元產生會導致資料庫多筆資料有優勢。

那麼雜湊衝突怎麼辦？即使是隨機字元方法，雖然機率較低，但衝突可能仍會發生，我們需要注意這點。若發生衝突，我們可能會在字串前後加上一些隨機值以避免衝突。

此外，選擇雜湊演算法很重要，我們需要分析不同演算法的 CPU 需求和特性，選出最適合的。

### 3. URL 是否有效？

給定一個要縮短的 URL，要如何驗證它是否有效？我們是否真的會驗證？最基本的檢查可以是用正規表達式驗證 URL 格式。更進一步的做法是嘗試開啟或訪問該 URL。但這裡有一些陷阱：

1. 我們需要定義成功的標準，例如 HTTP 200 表示有效。
2. 若該 URL 位於私有網路？
3. 若該 URL 暫時無法連線？

### 4. 資料儲存

最後是儲存。我們生成的資料要存到哪裡？有很多資料庫解決方案，我們需要挑選最適合此應用的。像 MySQL 等關聯式資料庫是合理的選擇，但**請務必參考 School of SRE 的 [SQL 資料庫章節](../databases_sql/intro.md) 及 [NoSQL 資料庫章節](../databases_nosql/intro.md) 以獲得更深入的決策參考。**

### 5. 其他

我們暫時不會考慮使用者管理或其他功能，如流量限制、自訂連結等，但這些功能日後可能會被添加，視需求而定。

以下為一個最簡可用的程式碼範例供參考，不過建議你自己嘗試實作。

```python
from flask import Flask, redirect, request
from hashlib import md5

app = Flask("url_shortener")

mapping = {}

@app.route("/shorten", methods=["POST"])
def shorten():
    global mapping
    payload = request.json

    if "url" not in payload:
        return "Missing URL Parameter", 400

    # TODO: 檢查 URL 是否有效

    hash_ = md5()
    hash_.update(payload["url"].encode())
    digest = hash_.hexdigest()[:5]  # 限制長度為 5 字元，限制越短衝突機率越高

    if digest not in mapping:
        mapping[digest] = payload["url"]
        return f"Shortened: r/{digest}\n"
    else:
        # TODO: 檢查雜湊衝突
        return f"Already exists: r/{digest}\n"


@app.route("/r/<hash_>")
def redirect_(hash_):
    if hash_ not in mapping:
        return "URL Not Found", 404
    return redirect(mapping[hash_])


if __name__ == "__main__":
    app.run(debug=True)

"""
執行結果:


===> 縮短

$ curl localhost:5000/shorten -H "content-type: application/json" --data '{"url":"https://linkedin.com"}'
Shortened: r/a62a4


===> 重新導向，注意回應碼 302 及 location 標頭

$ curl localhost:5000/r/a62a4 -v
* Uses proxy env variable NO_PROXY == '127.0.0.1'
*   Trying ::1...
* TCP_NODELAY set
* Connection failed
* connect to ::1 port 5000 failed: Connection refused
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> GET /r/a62a4 HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.1
> Accept: */*
>
* HTTP 1.0, assume close after body
< HTTP/1.0 302 FOUND
< Content-Type: text/html; charset=utf-8
< Content-Length: 247
< Location: https://linkedin.com
< Server: Werkzeug/0.15.4 Python/3.7.7
< Date: Tue, 27 Oct 2020 09:37:12 GMT
<
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<title>Redirecting...</title>
<h1>Redirecting...</h1>
* Closing connection 0
<p>You should be redirected automatically to target URL: <a href="https://linkedin.com">https://linkedin.com</a>.  If not click the link.
"""
```

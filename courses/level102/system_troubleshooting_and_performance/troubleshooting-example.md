在本節中，我們將看到一個問題範例並嘗試進行故障排除，最後會分享幾個由 LinkedIn 工程師早先分享過的著名故障排除故事。

### 範例 - 記憶體洩漏：
記憶體洩漏問題通常不易被察覺，直到服務運行一段時間（數天、數週甚至數月）後變得無響應，必須重新啟動服務或修復錯誤。在此類情況下，服務的記憶體使用量會在指標圖表中呈現持續增加的趨勢，大致會是如下面的圖表。

![](images/MemUsageChart.png)

記憶體洩漏是應用程式管理記憶體配置不當的情況，當不需要的記憶體未被釋放，隨著時間推移，物件會不斷累積在記憶體中，導致服務崩潰。一般來說，這些未釋放的物件會由[垃圾回收器](https://zh.wikipedia.org/wiki/%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6)自動處理，但有時候因為錯誤會失效。除錯有助於找出大量應用程式記憶體使用的地方，接著你可以開始追蹤並根據使用情況進行篩選。如果發現有不再使用但仍被引用的物件，可以透過刪除它們來避免記憶體洩漏。以 Python 應用程式為例，Python 內建有像是 [tracemalloc](https://docs.python.org/3/library/tracemalloc.html) 的功能，可以幫助追蹤物件首次被分配的位置。幾乎每種程式語言都有一套（內建或外部）工具或函式庫來協助找出記憶體問題。以 Java 為例，有著名的記憶體洩漏偵測工具 [Java VisualVM](http://visualvm.java.net/intro.html)。

接下來，我們來看一個帶有記憶體洩漏錯誤的簡單 Flask Web 應用程式，隨著每次請求，記憶體使用不斷增加，並示範如何使用 tracemalloc 來捕捉洩漏情況。

假設：已建立 Python 虛擬環境並安裝了 Flask。

**一個最簡單的帶錯誤 Flask 程式碼，請參考註解以了解更多資訊**  
![](images/FlaskCode.png)

**啟動 Flask 應用**  
![](images/FlaskStart.png)

**啟動時，記憶體使用約為 26576 KB，約 26MB**  
![](images/MemUsage01.png)

**隨著每次後續的 GET 請求，可以看到記憶體使用量慢慢增加。**  
![](images/MemUsage02.png)

**現在嘗試發送 10000 次請求，看看記憶體是否大幅增加。**  
為了達成大量請求，我們使用 Apache 的壓力測試工具 [“ab”](https://httpd.apache.org/docs/2.4/programs/ab.html)。發送 10000 次請求後，可以注意到 Flask 應用的記憶體使用幾乎增加了 15 倍，從初始的 **26576 KB 增加到 419316 KB，約從 26 MB 到 419 MB**，對於這麼簡單的網頁應用來說是很大的增幅。  
![](images/MemUsage03.png)

**接著使用 Python 的 [tracemalloc](https://docs.python.org/3/library/tracemalloc.html) 模組來理解應用程式的記憶體配置情況。** Tracemalloc 可以在特定時間點擷取記憶體快照，並對快照資料進行統計分析。

在 app.py 中加入最基本的程式碼，fetchuserdata.py 文件不做修改，這樣我們在每次訪問 /capture 路徑時都能捕捉 tracemalloc 快照。  
![](images/Tracemalloc01.png)

**重新啟動 app.py（flask run）後，操作流程如下：**  
- 首先訪問 http://127.0.0.1:5000/capture  
- 接著發送 10000 次訪問 http://127.0.0.1:5000/ ，讓記憶體洩漏累積。  
- 最後再訪問一次 http://127.0.0.1:5000/capture，擷取快照以判斷哪一行程式式使用的記憶體最多。  
![](images/Tracemalloc02.png)

在最終快照中，我們注意到分配最多記憶體的模組和行號，即 fetchuserdata.py 第 6 行；經過 10000 次請求，該處占用約 419 MB 記憶體。  
![](images/Tracemalloc03.png)

**總結**

以上範例展示了一個錯誤如何導致記憶體洩漏，以及如何使用 [tracemalloc](https://docs.python.org/3/library/tracemalloc.html) 來了解問題源頭。實際應用程序遠比此示範例複雜，使用 tracemalloc 可能會稍微影響應用性能，因為 tracemalloc 自身也存在開銷。請在生產環境中慎重使用。

如果你有興趣深入探討 Python 物件記憶體分配內部原理及記憶體洩漏的除錯，不妨參考 Sanket Patel 在 PyCon India 2019 的精彩演講 [Debug Memory Leak In Python Flask | Python Object Memory Allocation Internals](https://www.youtube.com/watch?v=s9kAghWpzoE)，LinkedIn 頁面：[Sanket Patel](https://www.linkedin.com/in/sanketplus/)。

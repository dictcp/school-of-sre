##

# 指令行工具
現今大多數 Linux 發行版都附帶一組用於監控系統效能的工具。這些工具幫助您測量並理解各種子系統統計資料（CPU、記憶體、網路等等）。讓我們來看看一些廣泛使用的工具。

-   **`ps/top`**：進程狀態指令 (`ps`) 顯示 Linux 系統中所有目前正在執行的進程資訊。`top` 指令與 `ps` 類似，但會定期更新顯示的資訊，直到程式終止為止。`top` 的進階版本稱為 `htop`，具有更友善的使用介面及一些額外功能。這些指令行工具附帶選項以修改指令的運作與輸出。以下是 `ps` 指令支援的一些重要選項：

    -   `-p <pid1, pid2,...>`：顯示匹配指定進程 ID 的進程資訊。同樣地，您也可以使用 `-u <uid>` 與 `-g <gid>` 來顯示屬於特定使用者或群組的進程資訊。

    -   `-a`：顯示其他使用者的進程資訊，以及自己的進程。

    -   `-x`：當顯示符合其他選項的進程時，包含不具控制終端的進程。

 ![top 指令結果](images/image12.png) 
 <p align="center"> 圖 2：top 指令結果 </p>

-   **`ss`**：socket 統計指令 (`ss`) 用於顯示系統上的網路 socket 資訊。這工具是已棄用的 [netstat](https://man7.org/linux/man-pages/man8/netstat.8.html) 的繼任者。以下是 `ss` 指令支援的一些選項：

    -   `-t`：顯示 TCP socket。同理，`-u` 顯示 UDP socket，`-x` 顯示 UNIX 領域 socket，依此類推。

    -   `-l`：只顯示監聽中的 socket。

    -   `-n`：命令指示不解析服務名稱，而是直接顯示埠號。

![系統中監聽 socket 列表](images/image8.png) <p align="center"> 圖 3：系統監聽 socket 列表 </p>

-   **`free`**：`free` 指令顯示主機的記憶體使用統計資料，如可用記憶體、已用記憶體與空閒記憶體。此指令常與 `-h` 選項搭配使用，以人類可讀格式顯示統計資料。

![主機記憶體統計（人類可讀格式）](images/image6.png) 
<p align="center"> 圖 4：主機記憶體統計（人類可讀格式） </p>

-   **`df`**：`df` 指令顯示磁碟空間使用統計資料。常搭配 `-i` 選項顯示 [inode](https://zh.wikipedia.org/wiki/Inode) 使用情況。`-h` 選項可用於以人類可讀格式顯示統計資料。

![系統磁碟空間使用統計（人類可讀格式）](images/image9.png) 
<p align="center"> 圖 5：系統磁碟空間使用統計（人類可讀格式） </p>

-   **`sar`**：`sar` 工具可即時監控多種子系統，如 CPU 與記憶體。可使用 `-o` 選項指定的檔案儲存資料，有助於識別異常情況。

-   **`iftop`**：介面流量指令 (`iftop`) 顯示主機在特定介面上的頻寬使用情況。此指令常用於識別活躍連線的頻寬使用。`-i` 選項指定監控的網路介面。

![主機活躍連線的網路頻寬使用](images/image2.png) 
<p align="center"> 圖 6：主機活躍連線的網路頻寬使用 </p>

-   **`tcpdump`**：`tcpdump` 指令是一款網路監控工具，可擷取網路上流動的封包並顯示封包描述。以下是部分可用選項：

    -   `-i <interface>`：監聽介面

    -   `host <IP/hostname>`：過濾指定主機的傳輸流量（含發送與接收）

    -   `src/dst`：顯示來源 (src) 或目的地 (dst) 的單向流量

    -   `port <port number>`：過濾特定埠的流量

![介面上的封包 tcpdump](images/image10.png) 
<p align="center"> 圖 7：主機上 <code>docker0</code> 介面的 <code>tcpdump</code> 封包 </p>

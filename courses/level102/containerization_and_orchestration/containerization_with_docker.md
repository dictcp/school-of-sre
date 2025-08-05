## 介紹

自 2013 年公開發佈以來，Docker 在眾多容器引擎中大受歡迎。以下是 Docker 如此受歡迎的一些原因：

- _提升了可攜性_

Docker 容器可以以 Docker 映像的形式，跨環境運行，無論是本機、內部部署還是雲端實例。相較之下，LXC 容器更依賴機器規格。

- _輕量化_

與虛擬機映像相比，Docker 映像非常輕量。例如，Ubuntu 18.04 的虛擬機映像大小約為 3GB，而 Docker 映像只有約 45MB！

- _容器映像的版本控制_

Docker 支持維護多個映像版本，便於查閱映像歷史，甚至進行回滾。

- _映像重用_

由於 Docker 映像是分層結構的，一個映像可以作為基礎，再在其上構建新的映像。例如，[Alpine](https://hub.docker.com/_/alpine) 是常用作基底的輕量映像（約 5MB）。Docker 使用[存儲驅動](https://docs.docker.com/storage/storagedriver/)管理這些層。

- _社群支持_

Docker Hub 是一個容器映像註冊中心，任何登入用戶都能上傳或下載容器映像。Docker Hub 中的熱門作業系統映像會定期更新，並有大量社群支持。

讓我們先來了解一些在討論 Docker 時常見的術語。

## Docker 術語

- _Docker 映像_

Docker 映像包含應用程式的可執行版本及其依賴（設定檔、函式庫、二進位檔），可獨立作為容器運行。它可以看作是一個容器的快照。  
Docker 映像是疊加在基底層之上的多層堆疊，這些層會被版本化。最新的版本會被用於基底映像之上。

`docker image ls` 列出主機上存在的映像。

- _Docker 容器_

Docker 容器是映像的執行實例。映像是靜態的，而基於映像創建的容器則可以執行和互動。這就是前面章節所說的「容器」。

`docker run` 是用來從映像建立容器的指令。  
`docker ps` 列出主機中目前正在運行的 Docker 容器。

- _Dockerfile_

這是一個純文字檔，內含組裝映像所需的指令，由 Docker 引擎（嚴格說是守護進程）執行。裡面包含基底映像、要注入的環境變數等資訊。

`docker build` 用於根據 Dockerfile 建構映像。

- _Docker Hub_

Docker 的官方映像註冊中心。任何用戶登入 Docker 後，皆可透過 `docker push` 上傳自訂映像，或用 `docker pull` 下載映像。

了解基本術語後，我們來看看 Docker 引擎的運作方式：CLI 指令如何被解釋、容器生命週期如何管理。

## Docker 引擎組件

以下是 Docker 引擎架構圖，幫助理解：

![Docker 引擎架構](images/dockerengine.png)

Docker 引擎採用用戶端-伺服器架構，包括三個組件：

- _Docker 用戶端_

這是使用者直接互動的組件。執行先前看過的 Docker 指令（如 push、pull、container ls、image ls）時，實際上是使用 Docker 用戶端。單一用戶端可與多個 Docker 守護程序溝通。

- _REST API_

提供 Docker 用戶端與守護程序間的通訊介面。

- _Docker 守護程序 (server)_

Docker 引擎的核心元件。它負責根據 Dockerfile 建構映像、從容器註冊伺服器拉取映像、將映像推送至註冊伺服器、啟動或停止容器等，並管理容器間的網路。

## 實作練習

官方的 [docker github](https://github.com/docker/labs) 提供多個等級的學習實作課程。我們推薦適合初學者的課程，請依序完成：

1. [本地環境設定](https://github.com/docker/labs/blob/master/beginner/chapters/setup.md)

2. [使用 Docker CLI 基礎](https://github.com/docker/labs/blob/master/beginner/chapters/alpine.md)

3. [建立並容器化簡易 Flask 應用](https://github.com/docker/labs/blob/master/beginner/chapters/webapps.md)

此外，這個 [初學者實作練習](https://github.com/docker/awesome-compose/tree/master/react-express-mongodb) 介紹如何 Docker 化 MERN（Mongo + React + Express）應用，也非常適合跟著練習。

## Docker 進階功能

我們討論完基礎的容器化及如何 Docker 化獨立應用，現實情況中，流程間需要互相溝通，尤其在微服務架構中尤為常見。

**Docker 網路**

Docker 網路促進同主機或跨主機容器間的互動。透過 `docker network` 指令，您可選擇容器與主機及其他容器互動的方式。  
例如，`host` 選項讓容器共用主機的網路堆疊；`bridge` 讓同主機上的容器間通訊，但不對外部開放；`overlay` 允許跨多主機的容器互動，前提是它們連接於同一網路；`macvlan` 則為容器分配獨立 MAC 位址，適用於傳統容器。  
這部分主題超出本模組範圍，建議參考官方 [docker networks](https://docs.docker.com/network/) 文件作深入了解。

**Volume（資料卷）**

除了映像、容器和網路，Docker 也提供在容器內創建與掛載資料卷的選項。通常，容器內的資料是暫存的——容器停止後資料會消失。Volume 可用於保存持久資料。  
如果您想練習使用 Volume，[這份 Docker 實作課程](https://dockerlabs.collabnix.com/beginners/volume/creating-volume-mount-from-dockercli.html) 是不錯的起點。

[下一章](https://linkedin.github.io/school-of-sre/level102/containerization_and_orchestration/orchestration_with_kubernetes/)將介紹如何使用 Kubernetes 來編排容器部署。

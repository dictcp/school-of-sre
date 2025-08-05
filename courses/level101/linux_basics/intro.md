# Linux 基礎知識

## 簡介  
### 先決條件

- 應該能熟悉使用任何作業系統，如 Windows、Linux，或  
- 預期具備作業系統的基本知識

## 課程期待學到的內容

本課程分為三個部分。第一部分涵蓋 Linux 作業系統的基礎知識。我們將討論 Linux 架構、Linux 發行版以及 Linux 作業系統的用途，也會介紹 GUI 和 CLI 之間的差異。

第二部分則介紹一些 Linux 的基本指令，我們會著重於用來瀏覽檔案系統、檢視及操作檔案、輸入/輸出重定向等的指令。

第三部分涵蓋 Linux 系統管理，包含 Linux 管理員日常操作的任務，例如使用者/群組管理、檔案權限管理、系統效能監控、日誌檔案處理等。

在第二部分和第三部分，我們會使用範例來幫助理解這些觀念。

## 課程未涵蓋部分

本課程不包含進階 Linux 指令及 Bash 指令腳本的內容，也不會涉及 Linux 內部細節。

## 課程內容

本課程涵蓋以下主題：

-  [Linux 簡介](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/)
    -  [什麼是 Linux 作業系統](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/#what-are-linux-operating-systems)
    -  [熱門 Linux 發行版有哪些](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/#what-are-popular-linux-distributions)
    -  [Linux 作業系統的用途](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/#uses-of-linux-operating-systems)
    -  [Linux 架構](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/#linux-architecture)
    -  [圖形使用者介面（GUI）與指令列介面（CLI）對比](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/#graphical-user-interface-gui-vs-command-line-interface-cli)
-  [指令列基礎](https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/)
    -  [實驗環境設定](https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/#lab-environment-setup)
    -  [什麼是指令](https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/#what-is-a-command)
    -  [檔案系統組織](https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/#file-system-organization)
    -  [瀏覽檔案系統的指令](https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/#commands-for-navigating-the-file-system)
    -  [操作檔案的指令](https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/#commands-for-manipulating-files)
    -  [檢視檔案的指令](https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/#commands-for-viewing-files)
    -  [Echo 指令](https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/#echo-command)
    -  [文字處理指令](https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/#text-processing-commands)
    -  [I/O 重定向](https://linkedin.github.io/school-of-sre/level101/linux_basics/command_line_basics/#io-redirection)
-  [Linux 系統管理](https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/)
    -  [實驗環境設定](https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/#lab-environment-setup)
    -  [使用者／群組管理](https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/#usergroup-management)
    -  [成為超級使用者](https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/#becoming-a-superuser)
    -  [檔案權限](https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/#file-permissions)
    -  [SSH 指令](https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/#ssh-command)
    -  [套件管理](https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/#package-management)
    -  [程序管理](https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/#process-management)
    -  [記憶體管理](https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/#memory-management)
    -  [守護程序與 Systemd](https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/#daemons)
    -  [日誌](https://linkedin.github.io/school-of-sre/level101/linux_basics/linux_server_administration/#logs)
-  [總結](https://linkedin.github.io/school-of-sre/level101/linux_basics/conclusion)
    -  [SRE 角色中的應用](https://linkedin.github.io/school-of-sre/level101/linux_basics/conclusion/#applications-in-sre-role)
    -  [有用的課程與教學](https://linkedin.github.io/school-of-sre/level101/linux_basics/conclusion/#useful-courses-and-tutorials)

## 什麼是 Linux 作業系統

大多數人熟悉的作業系統是 Windows，佔超過 75% 的個人電腦市場。Windows 作業系統基於 Windows NT 核心。

所謂的 _核心_ 是作業系統中最重要的部分，執行像是程序管理、記憶體管理、檔案系統管理等關鍵功能。

Linux 作業系統基於 Linux 核心。Linux 作業系統包含 Linux 核心、GUI/CLI、系統函式庫及系統工具。Linux 核心由 Linus Torvalds 獨立開發並發佈。Linux 核心是免費且開放原始碼的（請參考 [https://github.com/torvalds/linux](https://github.com/torvalds/linux)）。

Linux 是核心，而非完整作業系統。Linux 核心結合 GNU 系統可形成完整作業系統，因此 Linux 作業系統又稱作 GNU/Linux 系統。GNU 是一套廣泛的免費軟體集合，包括編譯器、除錯器、C 函式庫等（詳見 [Linux 與 GNU 系統](https://www.gnu.org/gnu/linux-and-gnu.en.html)）。

Linux 歷史 - [https://en.wikipedia.org/wiki/History_of_Linux](https://en.wikipedia.org/wiki/History_of_Linux)

## 什麼是熱門 Linux 發行版

Linux 發行版（_distro_）是基於 Linux 核心並搭配套件管理系統的作業系統。套件管理系統包含一系列工具，可幫助安裝、升級、設定及移除系統中的軟體。

軟體通常會針對特定發行版封裝成特定格式，並透過該發行版專屬的套件庫提供。操作系統透過套件管理器來安裝及管理這些軟體。

**熱門 Linux 發行版列表：**

- Fedora

- Ubuntu

- Debian

- CentOS

- Red Hat Enterprise Linux

- Suse

- Arch Linux


| 封裝系統             | 發行版                                      | 套件管理器
| -------------------- | ------------------------------------------ | -----------------
| Debian 風格（`.deb`） | Debian、Ubuntu                            | APT
| Red Hat 風格（`.rpm`）| Fedora、CentOS、Red Hat Enterprise Linux | YUM

## Linux 架構

![](images/linux/commands/image25.png)

- Linux 核心屬於單核心（monolithic）架構。

- 系統呼叫用於與 Linux 核心空間互動。

- 核心程式碼僅能在核心模式下執行，非核心程式碼則在使用者模式執行。

- 裝置驅動程式用來與硬體設備溝通。

## Linux 作業系統的用途

基於 Linux 核心的作業系統廣泛用於：

- 個人電腦

- 伺服器

- 手機 — Android 是基於 Linux 作業系統

- 嵌入式設備 — 手錶、電視、交通號誌等

- 衛星

- 網路設備 — 路由器、交換器等

## 圖形使用者介面（GUI）與指令列介面（CLI）

使用者藉由使用者介面與電腦互動，使用者介面可以是 GUI 或 CLI。

圖形使用者介面允許使用者透過圖形（如圖示和圖片）與電腦互動。當使用者點選圖示開啟應用程式時，實際上就是使用 GUI。GUI 操作直覺且簡單。

指令列介面則讓使用者透過輸入指令與電腦互動。使用者在終端機輸入指令，系統協助執行這些指令。對只熟悉 GUI 的新手而言，CLI 可能較難操作，因為需要瞭解各種指令與其用法。

## Shell 與 Terminal 的差異

Shell 是一個程式，負責接受使用者的指令並將它們交給作業系統處理。Shell 是 CLI（指令列介面）的範例。Bash 是 Linux 伺服器中最普及的 shell 程式之一，其他流行的 shell 包括 zsh、ksh 和 tcsh。

Terminal 是一個程式，可以開啟視窗讓使用者與 shell 互動。一些常見的終端機程式有 GNOME-terminal、xterm、Konsole 等。

Linux 使用者常會將 shell、terminal、prompt、console 等術語混用。簡單來說，這些都是用來接受使用者指令的介面方式。

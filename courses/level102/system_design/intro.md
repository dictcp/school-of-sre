# 系統設計

## 先決條件

- [School of SRE - 系統設計 - 第一階段](https://linkedin.github.io/school-of-sre/level101/systems_design/intro/)

## 這門課程的預期目標

本課程的目標是讓讀者能夠理解良好設計系統的基本構件、評估現有系統、理解取捨、提出自己的設計，並探索可用來實現該系統的各種工具。在本模組的第一階段，我們討論了系統設計的基礎，包括可擴展性、可用性和可靠性等概念。在本階段中，我們將繼續建立這些基礎。

<div class="callout callout-info">
 在整個課程中，會出現像這樣的提醒區塊，談論一些與系統設計流程密切相關但不屬於系統本身的內容。它們也會包含一些系統設計中常見問題的相關資訊，請特別留意這些內容。
</div>

## 本課程未涵蓋的範圍

雖然本課程涵蓋系統設計的許多方面，但並未涵蓋最基本的概念。對於那些主題，建議先閱讀先決條件。

一般而言，本模組不會介紹實際的架構落實——我們不會討論如何選擇主機／雲端供應商或編排設定，或持續整合／持續部署（CI/CD）系統。我們將著重於系統設計須考量的基本因素。

## 課程內容

- [介紹](https://linkedin.github.io/school-of-sre/level102/system_design/intro/)
- [大型系統設計](https://linkedin.github.io/school-of-sre/level102/system_design/large-system-design/)
- [擴展](https://linkedin.github.io/school-of-sre/level102/system_design/scaling/)
- [資料中心外的擴展](https://linkedin.github.io/school-of-sre/level102/system_design/scaling-beyond-the-datacenter/)
- [彈性的設計模式](https://linkedin.github.io/school-of-sre/level102/system_design/resiliency/)
- [結語](https://linkedin.github.io/school-of-sre/level102/system_design/conclusion/)

## 介紹

在本課程的前一階段，我們探討了如何建構一個基礎的照片分享應用。我們對該應用的基本需求是：

1. 能夠支援相當多的使用者
2. 在發生問題時，避免服務失效或整個叢集崩潰

換句話說，我們希望打造一個可用、可擴展且容錯的系統。我們將持續設計該應用，並在過程中涵蓋更多概念。

照片分享應用是一個網頁應用，處理用戶註冊、登入、上傳、動態產生、用戶互動及上傳內容互動等所有流程，同時需要一個資料庫來存儲這些資訊。在最簡單的設計中，網頁應用跟資料庫可以執行於同一台伺服器。請回想第一階段中的初始設計。

![第一個架構圖](images/initial_architecture.jpeg)


基於此，我們將談論系統設計中的效能元素——設定正確的效能衡量指標並以此驅動設計決策，透過快取、內容傳遞網路（CDN）等方式提升效能。同時，我們也會探討如何透過一些系統設計模式來強化彈性——例如優雅降級、逾時與斷路器。

<div class="callout callout-info">
<h4>成本</h4>
系統設計中像是可用性、可擴展性的考量不會孤立存在。在真實環境下，我們有其他考慮點／已有考慮點可能會呈現不同的面貌。其中一項即為成本。現實世界的系統幾乎總有預算限制。系統設計、實施與持續營運需要有可預測的每單位輸出成本，而輸出通常就是你試圖解決的商業問題。如何在兩者間取得平衡十分重要。
</div>

<div class="callout callout-primary">
<h4>理解系統的能力</h4>
一個良好設計的系統需要深入理解構件的能力。不是所有元件都是一樣的，了解單一元件的能力是非常重要的——例如，在照片上傳應用中，了解單一資料庫實例在每秒讀取或寫入交易量方面的能力，並合理預期數值，是必要的。這能幫助建立適當分配資源的系統，並排除明顯的瓶頸來源。
<br>
<br>
在較低層面，了解底層硬體（或雲端上的虛擬機實例）的能力也很重要。例如，不是所有磁碟的效能相同，也不是所有磁碟的每成本效能相同。如果預計 API 在正常情況下回應時間為 100 毫秒，重要的是要知道系統中各部分將花費多少時間。以下連結可幫助您了解從 CPU 快取到網路連線，直至終端用戶各環節的效能數據。
<br>
<br>
<a href="https://colin-scott.github.io/personal_website/research/interactive_latency.html">每個程式設計師都該知道的數字</a>
</div>

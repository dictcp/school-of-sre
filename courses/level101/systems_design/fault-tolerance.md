# 容錯能力

系統中的故障是無法避免且持續會發生的，因此我們需要建立能容忍故障或能從故障中恢復的系統。

- 在系統中，故障是常態而非例外。
- 「凡是可能出錯的事情終將出錯」——墨菲定律（Murphy’s Law）
- 「複雜系統內含有不斷變化的潛在故障混合」——How Complex Systems Fail。

### 容錯能力：故障衡量指標

任何系統中常被衡量與追蹤的故障指標。

**平均修復時間（MTTR，Mean time to repair）：** 修復並恢復故障系統的平均所需時間。

**平均故障間隔時間（MTBF，Mean time between failures）：** 裝置故障或系統故障與下一次故障之間的平均運行時間。

**平均故障時間（MTTF，Mean time to failure）：** 裝置或系統在發生故障前的平均可正常運作時間。

**平均偵測時間（MTTD，Mean time to detect）：** 問題發生與組織偵測到該問題的平均時間。

**平均調查時間（MTTI，Mean time to investigate）：** 從偵測事件到組織開始調查事件原因與解決方案之間的平均時間。

**平均恢復服務時間（MTRS，Mean time to restore service）：** 從事件偵測到受影響系統或組件重新可用於用戶的平均時間。

**平均系統事件間隔時間（MTBSI，Mean time between system incidents）：** 兩次連續事件偵測的平均時間間隔。MTBSI 可由 MTBF 加上 MTRS 計算（MTBSI = MTBF + MTRS）。

**故障率（Failure rate）：** 另一個可靠性指標，衡量元件或系統故障的頻率，以每單位時間內的故障數量表示。

#### 參考
- [https://www.splunk.com/en_us/data-insider/what-is-mean-time-to-repair.html](https://www.splunk.com/en_us/data-insider/what-is-mean-time-to-repair.html)

### 容錯能力：故障隔離術語
系統應該具備短路保護。舉例來說，在我們的內容分享系統裡，當「通知」功能失效時，網站應該能優雅地處理這個故障，移除該功能，而不是導致整個網站崩潰。

游泳道（Swimlane）是常用的故障隔離方法之一。游泳道為服務間增設防火牆，讓一方故障時不會影響另一方。舉例，我們在內容分享應用中推出新的「廣告」功能，我們可以有以下兩種架構。

![Swimlane](images/swimlane-1.jpg)

若廣告在每次新聞動態請求時同步即時產生，廣告功能的故障將會傳播至新聞動態功能。反之，如果我們將「廣告產生服務」游泳道隔離，並使用共享儲存來填充新聞動態應用，廣告故障將不會波及新聞動態，最壞情況下廣告未達服務水準協議（SLA），我們仍可繼續提供無廣告的新聞動態。

再舉一例，我們為內容分享應用設計了新模型，在此企業版內容分享應用中，企業付費使用，內容絕不會分享給企業外部。

![Swimlane-principles](images/swimlane-2.jpg)

### 游泳道原則

**原則一：** 不共享任何東西（也稱「盡可能少共享」）。游泳道內共享越少，故障隔離效果越佳。（如企業案例所示）

**原則二：** 不跨越游泳道邊界。同步通訊（以期望請求之形式定義，而非傳輸協定）不得跨越游泳道邊界；若有，表示邊界劃分錯誤。（如廣告功能所示）

### 游泳道方法
**方法一：** 將賺錢核心隔離為游泳道。絕不允許收銀系統被其他系統影響。（企業案例中的第 1 層對第 2 層）

**方法二：** 將主要的故障來源隔離為游泳道。辨識反覆發生的痛點並加以隔離。（若廣告功能呈現黃色警告，隔離廣告是最佳選擇）

**方法三：** 利用自然邊界隔離游泳道。客戶邊界是良好的游泳道劃分。（公開用戶對比企業用戶）

#### 參考
- [https://learning.oreilly.com/library/view/the-art-of/9780134031408/ch21.html#ch21](https://learning.oreilly.com/library/view/the-art-of/9780134031408/ch21.html#ch21)

### 在 SRE 角色的應用
1. 與資料中心技術團隊或雲端團隊合作，將基礎架構分佈在故障區域內，使其免受交換機或電力故障影響，建立資料中心內的容錯區域（[https://docs.microsoft.com/en-us/azure/virtual-machines/manage-availability#use-availability-zones-to-protect-from-datacenter-level-failures](https://docs.microsoft.com/en-us/azure/virtual-machines/manage-availability#use-availability-zones-to-protect-from-datacenter-level-failures)）。
2. 與合作夥伴共同設計服務間交互，確保單一服務故障不會以級聯方式放大影響至所有上游服務。

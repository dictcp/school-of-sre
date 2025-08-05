# 系統故障排除與效能提升

## 預備知識

* [Linux 基礎](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/)
* [系統設計](https://linkedin.github.io/school-of-sre/level101/systems_design/intro/)
* [基礎網路](https://linkedin.github.io/school-of-sre/level101/linux_networking/intro/)
* [指標與監控](https://linkedin.github.io/school-of-sre/level101/metrics_and_monitoring/introduction/)

## 課程期望

本短課程試圖提供一般性的介紹，說明如何排除系統問題，例如分析 API 故障、資源使用情況、網路問題、硬體與作業系統問題。課程也簡要介紹分析與基準測試，以評估整體系統性能。

## 課程不涵蓋內容

本課程不包含以下內容：

* 系統設計與架構。
* 程式設計實務。
* 指標與監控。
* 作業系統基礎。

## 課程內容
- [介紹](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/introduction)
- [故障排除](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/troubleshooting)
    - [故障排除流程圖](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/troubleshooting/#troubleshooting-flowchart)
    - [一般實務](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/troubleshooting/#general-practices)
    - [一般主機問題](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/troubleshooting/#general-host-issues)
- [重要工具介紹](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/important-tools)
    - [重要的 Linux 指令](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/important-tools/#important-linux-commands)
    - [日誌分析工具](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/important-tools/#log-analysis-tools)
- [效能提升](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/performance-improvements)
    - [效能分析指令](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/performance-improvements/#performance-analysis-commands)
    - [剖析工具](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/performance-improvements/#profiling-tools)
    - [基準測試](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/performance-improvements/#benchmarking)
    - [擴充](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/performance-improvements/#scaling)
- [故障排除範例](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/troubleshooting-example)
- [結論](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/conclusion)
    - [延伸閱讀](https://linkedin.github.io/school-of-sre/level102/system_troubleshooting_and_performance/conclusion/#further-readings)

## 介紹
故障排除是營運與開發中非常重要的一環。它不是只靠閱讀一篇文章或完成線上課程就能學會的，
而是一個持續學習的過程，在以下情況中逐漸習得：

* 日常營運與開發。
* 找出並修正應用程式錯誤。
* 找出並修正系統及網路問題。
* 性能分析與提升。
* 以及更多。

從 SRE 的角度來看，期望他們熟悉某些領域，以便能夠排查單一或分散式系統中的問題。

* 熟悉你的資源，了解主機規格，例如 CPU、記憶體、網路、磁碟等。
* 理解系統設計與架構。
* 確保重要指標被正確收集與呈現。

HP 創辦人曾有名言：「**測量即是改善的開始**」

如果系統元件與效能指標被徹底擷取，則在問題發生初期成功排除的機率將大幅提高。

### 範圍
針對不同類型的應用或服務沒有單一通用的故障排除方式，故障可能發生在任一層級。我們將本課程範圍限制於 Web API 服務類型。

**注意：** Linux 生態系廣泛，有數百種工具及實用程式可協助系統故障排除，每種工具擁有不同優點與功能。我們將涵蓋部分知名工具，這些工具存在於 Linux 系統或開源世界中。本文中提及工具的詳細說明不在本課程範圍內，請自行透過網路或 man 頁尋找更多範例與文件。

# 系統設計

## 先決條件

常見軟體系統元件的基礎知識：

- [Linux 基礎](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/)
- [Linux 網路](https://linkedin.github.io/school-of-sre/level101/linux_networking/intro/)
- 資料庫關聯式資料庫管理系統 (RDBMS)
- [NoSQL 概念](https://linkedin.github.io/school-of-sre/level101/databases_nosql/intro/)

## 本課程的學習重點

思考與設計大規模軟體系統的可擴展性、高可用性和可靠性。

## 課程不涵蓋範圍

個別軟體元件的可擴展性和可靠性議題，例如資料庫。雖然相同的可擴展性原則與思維可以套用，但這些個別元件在擴展與可靠性思考時各自有特定細節。

本課程會更著重於概念解析，而非設定與配置如負載平衡器等元件來達成系統的可擴展性、高可用性與可靠性。

## 課程內容

- [介紹](https://linkedin.github.io/school-of-sre/level101/systems_design/intro/#backstory)
- [可擴展性](https://linkedin.github.io/school-of-sre/level101/systems_design/scalability/)
- [高可用性](https://linkedin.github.io/school-of-sre/level101/systems_design/availability/)
- [容錯能力](https://linkedin.github.io/school-of-sre/level101/systems_design/fault-tolerance/)

## 介紹

那麼，該如何學習設計一個系統呢？

「*就像大多數偉大的問題一樣，這問題展現出令人驚嘆的天真程度。我唯一能給的簡短答案，基本上是你必須透過設計系統實際操作，才能學會如何設計系統，以及了解哪些方法有效哪些無效。*」— Jim Waldo，Sun 微系統，《系統設計論》

隨著軟體和硬體系統有多個運作元件，我們需要思考這些元件如何成長、它們的故障模式、彼此的相依關係，以及這些對使用者與業務的影響。

沒有一步到位的方法或技巧能教你系統設計，我們只能透過設計系統並不斷迭代中學習。

本課程旨在啟發您在系統設計時思考_可擴展性_、_高可用性_與_容錯能力_。

## 背景故事

讓我們設計一個簡單的內容分享應用程式，讓使用者能在應用中分享照片、媒體，並由朋友們點讚。我們先從簡單的應用架構設計開始，隨著學習系統設計概念將不斷演進。

![第一個架構圖](images/first-architecture.jpg)

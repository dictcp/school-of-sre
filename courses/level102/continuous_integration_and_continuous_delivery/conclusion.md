## SRE 角色中的應用

監控、自動化及消除繁瑣工作是 SRE 學科的一些核心支柱。作為 SRE，您可能需要花費約 50% 的時間在自動化重複性任務並消除繁瑣工作。CI/CD 管道是 SRE 的重要工具之一。它們有助於通過更小、更規律且更頻繁的構建來交付高品質的應用。此外，CI/CD 指標如部署時間、成功率、週期時間及自動測試成功率等，都是提升產品品質從而提高應用可靠性的關鍵指標。

* [基礎設施即代碼](https://en.wikipedia.org/wiki/Infrastructure_as_code) 是 SRE 中用來自動化重複配置任務的標準實踐之一。每個配置都維護為程式碼，因此可以通過 CI/CD 管道進行部署。將配置更改通過 CI/CD 管道交付至生產環境非常重要，以維護版本控制、環境間變更的一致性並避免人工錯誤。
* 作為 SRE，您經常需要審查應用的 CI/CD 管道，並建議增加例如靜態程式碼分析、安全與隱私檢查等階段，以提升產品的安全性和可靠性。

## 結論

本章我們研究了 CI/CD 管道，簡述了傳統構建實踐面臨的挑戰。我們還探討了 CI/CD 管道如何強化 SRE 學科。CI/CD 管道在軟體開發生命週期中的運用是 SRE 領域的一種現代方法，有助於實現更高的效率。

我們也進行了使用 Jenkins 創建 CI/CD 管道的實作實驗。

## 參考資料

1.	[持續整合 (martinfowler.com)](https://martinfowler.com/articles/continuousIntegration.html)
2.	[微服務的 CI/CD - Azure 架構中心 | Microsoft Docs](https://docs.microsoft.com/en-us/azure/architecture/microservices/ci-cd)
3.	[SRE 基礎藍圖 (devopsinstitute.com)](https://www.devopsinstitute.com/wp-content/uploads/2020/11/SREF-Blueprint.pdf)
4.	[Jenkins 使用者文件](https://www.jenkins.io/doc/)

***持續交付 (Continuous Delivery)*** 意指在非生產環境（如 [SIT、UAT、INT](https://medium.com/@buttertechn/qa-testing-what-is-dev-sit-uat-prod-ac97965ce4f)）更頻繁地部署應用程式建置，並自動執行整合測試與驗收測試。

在持續交付中，測試是針對整合後的應用程式進行，而非微服務架構中單一的微服務。測試必須包含所有功能測試與驗收測試，這些測試可能包含使用者介面（UI）測試。建置必須具有不可變更性，也就是說，相同的封裝必須在所有環境（包括生產環境）中部署。

部署至生產環境通常是在執行額外驗收測試（例如效能測試）後手動進行。因此，完全自動化部署至生產環境稱為 ***持續部署 (Continuous Deployment)***（而 ***CD – 持續交付 (Continuous Delivery)*** 並不會自動部署至生產環境）。持續部署必須具備 [功能開關 (feature toggle)](https://martinfowler.com/articles/feature-toggles.html)，使功能能夠在不需重新部署代碼的情況下被開啟或關閉。

通常，部署會涉及多個生產環境，例如在 [藍綠環境 (blue-green environments)](https://www.linkedin.com/pulse/using-blue-green-deployments-reduce-downtime-nessan-harpur) 中，應用程式會先部署至藍色環境，再部署至綠色環境，以避免停機時間。

![](./images/CD_Image1.JPG)

*圖 3：持續交付流程管線*

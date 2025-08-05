CI 是一種軟體開發實踐，團隊成員會頻繁整合他們的工作。每次整合都會由自動化構建（包括測試）來驗證，以便盡快偵測整合錯誤。

持續整合要求所有代碼變更都維護在單一代碼庫中，所有成員都可以定期將變更推送到他們的功能分支。代碼變更必須快速與其他代碼整合，自動化構建應該執行並反饋給成員，以便及早解決問題。

應該有一台 CI 伺服器，能在成員推送代碼後立即觸發構建。構建通常涉及編譯代碼並將其轉換為可執行檔案，例如 JAR 或 DLL 等，稱為打包。它還必須執行帶有代碼覆蓋率的[單元測試](https://zh.wikipedia.org/wiki/%E5%96%AE%E5%85%83%E6%B8%AC%E8%A9%A6)。構建過程還可以包含其他階段，如靜態代碼分析和漏洞檢查等。

[Jenkins](https://www.jenkins.io/)、[Bamboo](https://confluence.atlassian.com/bamboo/understanding-the-bamboo-ci-server-289277285.html)、[Travis CI](https://travis-ci.org/)、[GitLab](https://about.gitlab.com/)、[Azure DevOps](https://azure.microsoft.com/zh-tw/services/devops/) 等是幾個知名的 CI 工具。這些工具提供各種插件及整合，例如用於構建與打包的 [ant](https://ant.apache.org/)、[maven](https://maven.apache.org/) 等，以及用於執行單元測試的 Junit、selenium 等。[SonarQube](https://www.sonarqube.org/) 可用於靜態代碼分析與代碼安全檢查。

![](./images/CI_Image1.JPG)

*圖 1：持續整合流程圖*

![](./images/CI_Image2.JPG)

*圖 2：持續整合流程*

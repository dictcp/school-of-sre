我們了解最初建立的內容只是起點，我們希望社群能夠在這段旅程中協助精煉與擴充內容。

作為貢獻者，您保證您提交的內容並非抄襲。提交內容即表示您（以及在適用情況下您的雇主）同意將提交的內容依據「創用 CC 姓名標示 4.0 國際公共授權」授權給 LinkedIn 與開源社群。

*倉庫網址*: [https://github.com/linkedin/school-of-sre](https://github.com/linkedin/school-of-sre)

### 貢獻指南
請確保您遵守以下指南：

* 內容應關於可應用於任何公司或個人專案的原則與概念，不應聚焦於特定工具或技術棧（這些通常會隨時間改變）。
* 遵守 [行為準則](/school-of-sre/CODE_OF_CONDUCT/)。
* 內容應與 SRE 的角色與職責相關。
* 內容應在本地測試（請參照測試步驟）並且格式良好。
* 最佳實務是先開啟議題並討論您的更改，再提交拉取請求。如此您可以在開始前融合他人的建議。

### 本地構建與測試
在開啟 PR 前，執行以下指令以在本地構建並檢視網站。

```shell
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs build
mkdocs serve
```

### 開啟 PR
請依照 [GitHub PR 工作流程](https://guides.github.com/introduction/flow/) 進行您的貢獻。

將本倉庫分叉（fork），建立功能分支，提交更改，然後開啟指向本倉庫的拉取請求（PR）。

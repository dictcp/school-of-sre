### 重要的 Linux 指令

掌握以下指令將有助於更快地找出問題。詳細說明每個指令已超出範圍，請查閱 man 頁或線上資源以獲得更多資訊與範例。

- 用於解析日誌：grep、sed、awk、cut、tail、head
- 用於網路檢查：nc、netstat、traceroute/6、mtr、ping/6、route、tcpdump、ss、ip
- 用於 DNS：dig、host、nslookup
- 用於追蹤系統呼叫：strace
- 用於透過 ssh 並行執行：gnu parallel、xargs + ssh
- 用於 http/s 檢查：curl、wget
- 列出開啟檔案：lsof
- 修改系統核心屬性： [sysctl](https://man7.org/linux/man-pages/man8/sysctl.8.html)

若是分散式系統，某些第三方工具能協助您同時在多台主機上執行指令或操作，例如：

- **基於 SSH 的工具**
    - [ClusterSSH](https://github.com/duncs/clusterssh)：Cluster SSH 可幫您同時在多台主機執行指令。
    - [Ansible](https://github.com/ansible/ansible)：允許您撰寫 Ansible playbooks，能在數百甚至數千台主機同時執行。
- **基於 Agent 的工具**
    - [Saltstack](https://github.com/saltstack/salt)：是一個配置、狀態及遠端執行框架，為使用者提供各種靈活性以同時在大量主機執行模組。
    - [Puppet](https://github.com/puppetlabs/puppet)：自動化管理引擎，支援 Linux、Unix 及 Windows 系統，執行管理任務。

### 日誌分析工具

這些工具可協助撰寫類似 SQL 的查詢來解析與分析日誌，並提供簡易的 UI 介面以建立儀表板，能根據定義的查詢呈現各類圖表。

- [ELK](https://www.elastic.co/what-is/elk-stack)：Elasticsearch、Logstash 與 Kibana，提供一套工具與服務，可輕鬆快速地解析日誌、建立索引並分析日誌。一旦日誌/資料經由 Logstash 解析過濾並在 Elasticsearch 中建立索引，便可在 Kibana 中於數分鐘內建立動態儀表板，有助於輕鬆分析與關聯應用錯誤/例外/警告。
- [Azure Kusto](https://docs.microsoft.com/en-us/azure/data-explorer)：Azure Kusto 是雲端服務，類似 Elasticsearch 與 Kibana，允許輕鬆建立大量日誌索引，提供類 SQL 查詢介面及創建動態儀表板的功能。

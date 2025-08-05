# 結論

我們已經涵蓋了 Linux 作業系統的基礎知識以及在 Linux 中常用的基本指令。  
我們也介紹了 Linux 伺服器管理的指令。

希望這門課程能讓你更輕鬆地使用指令列操作。

## 在 SRE 角色中的應用

1. 作為 SRE，你需要在這些 Linux 伺服器上執行一些常見任務。在排解問題時，你也會使用指令列。
2. 在檔案系統中從一個位置移動到另一個位置會需要使用 `ls`、`pwd` 和 `cd` 指令的協助。
3. 你可能需要在日誌檔案中搜尋特定資訊，`grep` 指令非常好用。如果你想將輸出存到檔案，或者當作輸入傳給另一個指令，I/O 重導向會很實用。
4. `tail` 指令非常適合用來查看日誌檔案中的最新資料。
5. 不同使用者根據角色會有不同的權限。為了安全考量，我們也不希望公司所有人都能存取伺服器。使用者權限可以用 `chown`、`chmod` 和 `chgrp` 指令來限制。
6. 對 SRE 而言，`ssh` 是最常使用的指令之一。登入伺服器並進行排錯及基本管理工作，必須先能成功登入伺服器。
7. 如果想在伺服器上執行 Apache 或 NGINX，我們會先使用套件管理器安裝。套件管理指令在這方面就非常重要。
8. 管理伺服器上的服務是 SRE 的另一項重要責任。`systemd` 相關指令能幫忙排錯。服務中斷時，我們可以用 `systemctl start` 指令啟動它，也可以在不需要時停止服務。
9. 監控是 SRE 的核心職責之一。記憶體和 CPU 是兩個重要的系統層級指標，應該持續監控。像是 `top` 和 `free` 指令相當有幫助。
10. 當服務拋出錯誤時，我們如何找出錯誤根本原因？肯定需要查看日誌來找出完整的錯誤堆疊軌跡。日誌檔也會告訴我們錯誤發生次數以及開始時間。

## 有用的課程與教學

* [Edx 基本 Linux 指令課程](https://courses.edx.org/courses/course-v1:LinuxFoundationX+LFS101x+1T2020/course/)
* [Edx 紅帽企業 Linux 課程](https://courses.edx.org/courses/course-v1:RedHat+RH066x+2T2017/course/)
* [https://linuxcommand.org/lc3_learning_the_shell.php](https://linuxcommand.org/lc3_learning_the_shell.php)

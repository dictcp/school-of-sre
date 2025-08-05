
# Linux-中階

## 先決條件

- 預計已完成 School Of SRE [<u>Linux 基礎</u>](https://linkedin.github.io/school-of-sre/level101/linux_basics/intro/) 課程。

## 本課程預期學習內容

 本課程分為兩個部分。第一部分將從先前 School of SRE 課程中的 Linux 基礎內容繼續，深入探討一些較進階的 Linux 指令與概念。
 
 第二部分則會講解如何在日常工作中，透過 Bash 腳本自動化及減少重複性工作，並輔以實際 SRE 的案例說明。

## 本課程不涵蓋的內容

本課程旨在讓你熟悉 Linux 指令、shell 腳本與 SRE 實務的交集，不會涵蓋 Linux 核心內部結構。

## 實驗環境設定

- 請在你的系統安裝 Docker。 [<u>https://docs.docker.com/engine/install/</u>](https://docs.docker.com/engine/install/)
    
- 我們將使用 RedHat Enterprise Linux (RHEL) 8。

![](images/image1.png)

- 大部分指令將在上述 Docker 容器中執行。

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

## 課程內容

[<u>套件管理</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/package_management/)

- [<u>套件 (Package)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/package_management/#package)
    
- [<u>依賴 (Dependencies)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/package_management/#dependencies)
    
- [<u>軟體庫 (Repository)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/package_management/#repository)
    
- [<u>高階與低階套件管理工具</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/package_management/#high-level-and-low-level-package-management-tools)
    

[<u>儲存媒體</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/)

- [<u>列出已掛載的儲存裝置</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#listing-the-mounted-storage-devices)
    
- [<u>建立檔案系統</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#creating-a-filesystem)
    
- [<u>掛載裝置</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#mounting-the-device)
    
- [<u>卸載裝置</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#unmounting-the-device)
    
- [<u>使用 /etc/fstab 文件簡化掛載工作？</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#making-it-easier-with-etcfstab-file)
    
- [<u>檢查與修復檔案系統</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#checking-and-repairing-fs)
    
- [<u>RAID</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#raid)
    
    - [<u>RAID 等級</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#raid-levels)
        
    - [<u>RAID 0 (條帶化)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#raid-0-striping)
        
    - [<u>RAID 1 (鏡像)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#raid-1mirroring)
        
    - [<u>RAID 5 (條帶與分散式奇偶校驗)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#raid-5striping-with-distributed-parity)
        
    - [<u>RAID 6 (條帶與雙重奇偶校驗)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#raid-6striping-with-double-parity)
        
    - [<u>RAID 10 (RAID 1+0：鏡像與條帶化)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#raid-10raid-10-mirroring-and-striping)
        
    - [<u>監控 RAID 的指令</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#commands-to-monitor-raid)
        
- [<u>邏輯卷管理 (LVM)</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/storage_media/#lvm)
    

[<u>檔案打包與備份</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/)

- [<u>檔案打包</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/#archiving)
    
    - [<u>gzip</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/#gzip)
        
    - [<u>tar</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/#tar)
        
    - [<u>建立包含檔案與資料夾的打包檔案</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/#create-an-archive-with-files-and-folder)
        
    - [<u>列出打包檔案中的檔案列表</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/#listing-files-in-the-archive)
        
    - [<u>從打包檔案中解壓檔案</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/#extract-files-from-the-archive)
        
- [<u>備份</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/#backup)
    
    - [<u>增量備份</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/#incremental-backup)
        
    - [<u>差異備份</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/#differential-backup)
        
    - [<u>網路備份</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/#network-backup)
        
    - [<u>雲端備份</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/archiving_backup/#cloud-backup)
        
[<u>Vim 入門</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/introvim/)
 
- [<u>開啟檔案與使用插入模式</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/introvim/#opening-a-file-and-using-insert-mode)
    
- [<u>儲存檔案</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/introvim/#saving-a-file)
    
- [<u>離開 VIM 編輯器</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/introvim/#exiting-the-vim-editor)
    
[<u>Bash 腳本</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting/)

- [<u>撰寫第一個 Bash 腳本</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting/#writing-the-first-bash-script)
    
- [<u>接收使用者輸入並使用變數</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting/#taking-user-input-and-working-with-variables)
    
- [<u>退出狀態</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting/#exit-status)
    
- [<u>指令列參數與 If … else 條件分支</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting/#command-line-arguments-and-understanding-if-..-else-branching)
    
- [<u>迴圈實現重複任務</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting/#looping-over-to-do-a-repeated-task.)
    
- [<u>函式</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/bashscripting/#function)
    
[<u>課程總結</u>](https://linkedin.github.io/school-of-sre/level102/linux_intermediate/conclusion/)
        


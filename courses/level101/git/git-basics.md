# Git

## 先決條件

1. 已安裝 Git [https://git-scm.com/downloads](https://git-scm.com/downloads)
2. 已學習過任何 git 高階教程或正在跟隨 LinkedIn Learning 課程
      - [https://www.linkedin.com/learning/git-essential-training-the-basics/](https://www.linkedin.com/learning/git-essential-training-the-basics/)
      - [https://www.linkedin.com/learning/git-branches-merges-and-remotes/](https://www.linkedin.com/learning/git-branches-merges-and-remotes/)
      - [官方 Git 文件](https://git-scm.com/doc)

## 從這門課程中可以期待什麼

作為計算機科學領域的工程師，具有版本控制工具的知識幾乎是必備的。雖然市面上存在許多版本控制工具，例如 SVN、Mercurial 等，但 Git 或許是最常用的一個，而本課程將使用 Git。雖然本課程不會從 Git 101 開始，並預期學員已有基本的 git 知識，課程會重新介紹你已知的 git 概念，並詳細說明在執行各種 `git` 命令時底層發生了什麼。這樣，下次你執行 `git` 命令時，就能更加自信地按下 `enter`！

## 本課程未涵蓋的內容

Git 的進階使用與內部實作細節。

## 課程內容

 1. [Git 基礎](https://dictcp.github.io/school-of-sre/level101/git/git-basics/#git-basics)
 2. [使用分支](https://dictcp.github.io/school-of-sre/level101/git/branches/)
 3. [Git 與 Github](https://dictcp.github.io/school-of-sre/level101/git/github-hooks/#git-with-github)
 4. [Hooks](https://dictcp.github.io/school-of-sre/level101/git/github-hooks/#hooks)

## Git 基礎

即使你可能已經知道，讓我們再回顧為什麼我們需要版本控制系統。隨著專案成長，多名開發者開始協作，一個高效的協作方式就變得必要。Git 讓團隊能輕鬆協作，同時也維護程式碼庫變更的歷史。

### 建立 Git 倉庫

任何資料夾都可以轉換成 git 倉庫。執行以下命令後，我們會看到資料夾內多了一個 `.git` 資料夾，這就是使我們的資料夾成為 git 倉庫的關鍵。**git 所有的魔法，都是由 `.git` 資料夾促成。**

```bash
# 建立空資料夾並切換到該目錄
$ cd /tmp
$ mkdir school-of-sre
$ cd school-of-sre/

# 初始化 git 倉庫
$ git init
Initialized empty Git repository in /private/tmp/school-of-sre/.git/
```

輸出顯示我們的資料夾內已初始化空白 git 倉庫。讓我們來看看內容：

```bash
$ ls .git/
HEAD        config      description hooks       info        objects     refs
```

`.git` 內有許多資料夾與檔案。正如我所說，這些讓 git 能魔法般運作。我們會介紹這些資料夾和檔案。不過現在，我們擁有的是一個空的 git 倉庫。

### 追蹤檔案

如你所知，先在倉庫（以下用 _repo_ 稱呼此資料夾）建立新檔案，並查看 `git status`：

```bash
$ echo "I am file 1" > file1.txt
$ git status
On branch master

No commits yet

Untracked files:
 (use "git add <file>..." to include in what will be committed)

       file1.txt

nothing added to commit but untracked files present (use "git add" to track)
```

目前 git 狀態顯示「No commits yet」且有一個未追蹤檔案。因為我們剛建立檔案，git 尚未追蹤它。我們必須明確告訴 git 追蹤此檔案。（也可以參考 [gitignore 文件](https://git-scm.com/docs/gitignore)）方法是透過 `git add` 命令，如上面輸出建議。然後，我們進行提交。

```bash
$ git add file1.txt
$ git status
On branch master

No commits yet

Changes to be committed:
 (use "git rm --cached <file>..." to unstage)

       new file:   file1.txt

$ git commit -m "adding file 1"
[master (root-commit) df2fb7a] adding file 1
1 file changed, 1 insertion(+)
create mode 100644 file1.txt
```

注意加入檔案後，`git status` 會顯示`Changes to be committed:`。這表示列表中所列的檔案將包含在下一次提交中。接著，我們透過 `-m` 參數附加訊息，完成提交。

### 關於提交（Commit）更多資訊

Commit 是 repo 的一個快照。每當執行提交時，都會將目前 repo（此資料夾）狀態拍照並且保存。每個提交都有唯一 ID（例如前面步驟中我們產生的 commit ID 為 `df2fb7a`）。隨著我們持續新增/更改檔案並提交，git 儲存所有這些快照。所有魔法都是發生在 `.git` 資料夾中，所有快照版本都以高效率方式儲存在裡面。

### 添加更多變更

我們再建立一個檔案並提交變更，其操作與之前類似：

```bash
$ echo "I am file 2" > file2.txt
$ git add file2.txt
$ git commit -m "adding file 2"
[master 7f3b00e] adding file 2
1 file changed, 1 insertion(+)
create mode 100644 file2.txt
```

這次提交產生一個新 commit ID 為 `7f3b00e`。你可以隨時使用 `git status` 查看倉庫狀態。

       **重要：請注意 commit ID 是很長的字串（SHA），但我們也可用它們的前幾個字元（8 個或更多）來參考。我們會交替使用較短和完整的 commit ID。**

現在我們有兩個 commit，來視覺化它們：

```bash
$ git log --oneline --graph
* 7f3b00e (HEAD -> master) adding file 2
* df2fb7a adding file 1
```

`git log` 如其名，列出所有 git 提交記錄。這裡用了兩個參數，`--oneline` 會列出簡短版本，只顯示 commit 訊息，不包含提交者和時間。`--graph` 則以圖形化方式呈現。

**目前看起來提交都是一行一筆，不過 git 內部是以樹狀資料結構儲存所有 commit。意思是單一 commit 可以有兩個或以上的子 commit，不會只有一條直線的 commit 歷史。我們會在分支章節深入探討。現在，我們的 commit 歷史是：**

```bash
   df2fb7a ===> 7f3b00e
```

### commit 真的是相互連結的嗎？

正如前面所說，我們的兩個 commit 是以樹狀結構互連，並知道如何連結。現在我們來驗證它。git 中的一切都是物件。新建立的檔案是以物件儲存，檔案的變更也是物件，連提交本身也是物件。要查看物件內容，可以用該物件的 ID 執行以下命令。我們來看第二個提交的內容：

```bash
$ git cat-file -p 7f3b00e
tree ebf3af44d253e5328340026e45a9fa9ae3ea1982
parent df2fb7a61f5d40c1191e0fdeb0fc5d6e7969685a
author Sanket Patel <spatel1@linkedin.com> 1603273316 -0700
committer Sanket Patel <spatel1@linkedin.com> 1603273316 -0700

adding file 2
```

請注意輸出中 `parent` 屬性，它指向我們前一個提交的 commit ID。這證明兩者確實連結！另外你也看到該 commit 的訊息。所有這些魔法都由 `.git` 資料夾驅動，而此物件也儲存在裡面。

```bash
$ ls .git/objects/7f/3b00eaa957815884198e2fdfec29361108d6a9
.git/objects/7f/3b00eaa957815884198e2fdfec29361108d6a9
```

物件被儲存在 `.git/objects/` 資料夾。所有檔案及變更皆存在此資料夾。

### Git 的版本控制部分

從 git log 已可查看兩個提交（兩個版本）。版本控制工具最重要的特點之一是可以在歷史中前後切換。例如：部分用戶在使用舊版程式碼時發現問題並回報。要除錯問題，你必須拿到舊版程式碼。你目前 repo 是最新版本。在此例中，你正在第二個 commit（`7f3b00e`）上工作，而有人報告第一個 commit (`df2fb7a`) 的程式碼有問題。此時你可以透過以下方法取得舊版本的程式碼：

```bash
# 目前目錄中有兩個檔案
$ ls
file1.txt file2.txt

# 切換到舊的 commit
$ git checkout df2fb7a
Note: checking out 'df2fb7a'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

 git checkout -b <new-branch-name>

HEAD is now at df2fb7a adding file 1

# 查看目錄，確認為舊狀態檔案
$ ls
file1.txt
```

這就是如何取得舊版本/快照。你只需要該快照的 _參考_。執行 `git checkout ...` 時，git 會透過 `.git` 資料夾查看該版本檔案狀態，並將當前目錄中內容替換成該版本快照。當時存在的檔案會不再顯示於本地，但因為被 `git commit` 追蹤且儲存在 `.git`，仍然可取得。

### 參考（References）

前面提過，我們需要一個 _參考_ 指向某個版本。預設 git 倉庫是由 commit 樹組成，每個 commit 有唯一 ID，但這不是唯一能用來參考 commit 的方式。還有多種方式參考 commit。例：

- `HEAD` 是目前 commit 的參考。_不論你的 repo checked out 在哪個 commit，HEAD 都會指向它。_
- `HEAD~1` 是上一個 commit 的參考。因此在前面切換上一個版本時，可用 `git checkout HEAD~1`。

同樣地，`master` 也是一個參考（分支名稱）。由於 git 以樹狀結構儲存提交，當然會有分支。預設分支叫做 `master`。master（或其他分支名稱）會指向該分支的最新 commit。即使我們切換過去看舊 commit，`master` 仍指向最新 commit。我們可以透過切換回 `master` 取得最新版本。

```bash
$ git checkout master
Previous HEAD position was df2fb7a adding file 1
Switched to branch 'master'

# 現在可以看到兩個檔案的最新程式碼
$ ls
file1.txt file2.txt
```

註：上述指令中，也可用 commit ID 取代 master。

### 參考與魔法

讓我們來看目前狀態。兩個 commit，`master` 與 `HEAD` 都指向最新 commit。

```bash
$ git log --oneline --graph
* 7f3b00e (HEAD -> master) adding file 2
* df2fb7a adding file 1
```

魔法在哪？我們來檢查這些檔案：

```bash
$ cat .git/refs/heads/master
7f3b00eaa957815884198e2fdfec29361108d6a9
```

瞧！`master` 指向的 commit ID 就存放在一個檔案中。**當 git 需要知道 master 指向哪裡，或要更新 master 指向的 commit，只要讀取或寫入此檔案即可。**新增提交時，git 在當前 commit 上加入新 commit，並更新 master 檔案內容為新 commit ID。

同理，`HEAD` 參考：

```bash
$ cat .git/HEAD
ref: refs/heads/master
```

`HEAD` 指向另一個參考 `refs/heads/master`。因此 `HEAD` 所指即是 `master` 指向的位置。

### 小小冒險

我們討論過 git 執行命令時會修改這些檔案。但我們自己手動操作看看會怎樣。

```bash
$ git log --oneline --graph
* 7f3b00e (HEAD -> master) adding file 2
* df2fb7a adding file 1
```

現在，把 `master` 指向改為第一個 commit。

```bash
$ echo df2fb7a61f5d40c1191e0fdeb0fc5d6e7969685a > .git/refs/heads/master
$ git log --oneline --graph
* df2fb7a (HEAD -> master) adding file 1

# 恢復原本指向
$ echo 7f3b00eaa957815884198e2fdfec29361108d6a9 > .git/refs/heads/master
$ git log --oneline --graph
* 7f3b00e (HEAD -> master) adding file 2
* df2fb7a adding file 1
```

我們剛剛編輯了 `master` 參考的檔案，因此 `git log` 只顯示了第一筆提交。還原檔案後狀態回到原本。其實也沒什麼神奇的。

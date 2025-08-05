# 使用分支

回到我們有兩個提交的本地倉庫。到目前為止，我們有的是一條單線的歷史記錄。提交被串聯成單一線條。但有時候，你可能需要在同一倉庫裡同時平行開發兩個不同的功能。這時候一個選項可能是複製一個資料夾／倉庫，有同樣的程式碼，來做另一個功能的開發。但有更好的方法，那就是使用 _分支_（branches）。因為 git 的提交結構是樹狀的，我們可以利用分支來同時處理不同的功能集。從同一個提交點，可以創建兩個或更多分支，分支之間也能合併。

使用分支，多條歷史線路可以同時存在，我們可以切換（checkout）到任一條並進行作業。正如我們之前討論的，checkout 就是用所切換版本的快照替換目錄（倉庫）內容。

來建立一個分支看起來會是這樣：

```bash
$ git branch b1
$ git log --oneline --graph
* 7f3b00e (HEAD -> master, b1) adding file 2
* df2fb7a adding file 1
```

我們建立了一個名為 `b1` 的分支。git log 告訴我們 `b1` 也指向最後一個提交（`7f3b00e`），但 `HEAD` 仍指向 `master`。如果你還記得，`HEAD` 指向你目前檢出的提交／參考點。所以如果我們切換到 `b1`，`HEAD` 就會指向它。讓我們確認一下：

```bash
$ git checkout b1
Switched to branch 'b1'
$ git log --oneline --graph
* 7f3b00e (HEAD -> b1, master) adding file 2
* df2fb7a adding file 1
```

`b1` 仍指向同一個提交，但 `HEAD` 現在指向 `b1`。因為我們是在提交 `7f3b00e` 建立分支，所以從這裡開始就有兩條歷史線路。你檢出到哪個分支，歷史線路就往哪邊發展。

此時，我們檢出在 `b1` 分支，若做新提交，分支指標 `b1` 會往前移動到新提交，而之前 `b1` 指向的提交即成為新提交的父提交。試試看。

```bash
# 新增文件並提交
$ echo "I am a file in b1 branch" > b1.txt
$ git add b1.txt
$ git commit -m "adding b1 file"
[b1 872a38f] adding b1 file
1 file changed, 1 insertion(+)
create mode 100644 b1.txt

# 查看新歷史線路
$ git log --oneline --graph
* 872a38f (HEAD -> b1) adding b1 file
* 7f3b00e (master) adding file 2
* df2fb7a adding file 1
$
```

請注意，master 仍然指向它原本的提交。我們現在可以切換回 `master` 分支並繼續那邊的提交，這樣會有另一條從 `7f3b00e` 分出的歷史線路。

```bash
# 切換回 master 分支
$ git checkout master
Switched to branch 'master'

# 在 master 分支建立一個新提交
$ echo "new file in master branch" > master.txt
$ git add master.txt
$ git commit -m "adding master.txt file"
[master 60dc441] adding master.txt file
1 file changed, 1 insertion(+)
create mode 100644 master.txt

# 查看歷史線路
$ git log --oneline --graph
* 60dc441 (HEAD -> master) adding master.txt file
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

注意這裡因為我們在 `master` 分支，所以看不到 `b1`。試著把兩個分支的歷史都看出來：

```bash
$ git log --oneline --graph --all
* 60dc441 (HEAD -> master) adding master.txt file
| * 872a38f (b1) adding b1 file
|/
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

上述的樹狀結構會讓你更清楚。你可以看到在提交 `7f3b00e` 有明顯的分支／分岔。這就是分支的建立方式。現在它們是兩條獨立的歷史線，可以獨立開發功能。

**重申一次：底層來說，git 就是一個提交的樹狀結構。分支名稱（人類可讀）是樹上指向某個提交的指標。我們透過各種 git 指令操作這個樹和指標，git 就會相應地修改倉庫內容。**

## 合併（Merges）

假設你在 `b1` 分支完成了你的功能，現在想把它合併回 `master` 分支，`master` 存放的是最終版本的程式碼。首先你會切換（checkout）回 `master` 分支，從遠端（例如 GitHub）拉取（pull）最新程式碼，接著要把 `b1` 的程式碼合併回 `master`。有兩種方法可行。

目前的歷史是：

```bash
$ git log --oneline --graph --all
* 60dc441 (HEAD -> master) adding master.txt file
| * 872a38f (b1) adding b1 file
|/
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

**方法一：直接合併分支。** 把 `b1` 合併到 `master`，會產生一個新的合併提交。它會合成兩條歷史線的變更，建立一個結果提交。

```bash
$ git merge b1
Merge made by the 'recursive' strategy.
b1.txt | 1 +
1 file changed, 1 insertion(+)
create mode 100644 b1.txt
$ git log --oneline --graph --all
*   8fc28f9 (HEAD -> master) Merge branch 'b1'
|\
| * 872a38f (b1) adding b1 file
* | 60dc441 adding master.txt file
|/
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

你會看到新產生了一個合併提交 (`8fc28f9`)。你會被提示輸入合併提交訊息。如果倉庫中有很多分支，這會產生大量合併提交，歷史看起來會很亂，不像單線歷史那麼乾淨明瞭。接著我們看另一種方案。

先把剛才的合併 reset 回前狀態：

```bash
$ git reset --hard 60dc441
HEAD is now at 60dc441 adding master.txt file
$ git log --oneline --graph --all
* 60dc441 (HEAD -> master) adding master.txt file
| * 872a38f (b1) adding b1 file
|/
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

**方法二：Rebase。** 現在，不合併兩個分支，而是把 `b1` 變基到 `master` 上。意即把分支 `b1`（從提交 `7f3b00e` 到 `872a38f`）移動到 (`60dc441`，也就是 `master` 頂端) 之上。

```bash
# 切換到 b1
$ git checkout b1
Switched to branch 'b1'

# 把 b1 重新變基到 master
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: adding b1 file

# 結果
$ git log --oneline --graph --all
* 5372c8f (HEAD -> b1) adding b1 file
* 60dc441 (master) adding master.txt file
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

你可以看到 `b1` 原本的提交父提交是 `7f3b00e`，經過 rebase 後，父提交變成了 `60dc441` （master 頂端）。這樣做使歷史變成單線。如果接著合併 `b1` 到 `master`，就只是把 `master` 指向 `5372c8f`（b1）。試試看：

```bash
# 切換到 master 準備合併
$ git checkout master
Switched to branch 'master'

# 現在的歷史（b1 以 master 為基底）
$ git log --oneline --graph --all
* 5372c8f (b1) adding b1 file
* 60dc441 (HEAD -> master) adding master.txt file
* 7f3b00e adding file 2
* df2fb7a adding file 1


# 执行合并，注意會看到 "fast-forward" 提示
$ git merge b1
Updating 60dc441..5372c8f
Fast-forward
b1.txt | 1 +
1 file changed, 1 insertion(+)
create mode 100644 b1.txt

# 合併後結果
$ git log --oneline --graph --all
* 5372c8f (HEAD -> master, b1) adding b1 file
* 60dc441 adding master.txt file
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

現在你看到 `b1` 和 `master` 都指向同一個提交。你的程式碼已合併到 master 分支，可以推送（push）了。歷史線路也很乾淨！:D

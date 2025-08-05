# 使用 Git 與 GitHub

到目前為止，我們所做的所有操作都是在本地的倉庫中進行，而 Git 也幫助我們在協作環境中工作。GitHub 是網際網路上的一個地方，你可以在那裡集中託管你的 Git 倉庫，並與其他開發者協作。

大多數工作流程會與之前討論的一樣，附加了幾件事：

 1. Pull（拉取）：從 GitHub（中央）倉庫拉取最新的變更
 2. Push（推送）：將你的變更推送到 GitHub 倉庫，讓所有人都可以取得

GitHub 已經撰寫了很棒的指南與教學，你可以參考：

- [GitHub Hello World](https://guides.github.com/activities/hello-world/)
- [Git Handbook](https://guides.github.com/introduction/git-handbook/)

## Hooks（掛勾）

Git 有另一個很棒的功能叫做 hooks（掛勾）。Hooks 基本上是當特定事件發生時會被調用的腳本。hooks 位於這裡：

```bash
$ ls .git/hooks/
applypatch-msg.sample     fsmonitor-watchman.sample pre-applypatch.sample     pre-push.sample           pre-receive.sample        update.sample
commit-msg.sample         post-update.sample        pre-commit.sample         pre-rebase.sample         prepare-commit-msg.sample
```

名稱不言自明。當特定事件發生時，如果你想執行某些動作，這些 hooks 就很有用。比如你想在推送程式碼前先執行測試，你可以設定 `pre-push` hook。現在讓我們試著創建一個 pre-commit hook。

```bash
$ echo "echo this is from pre commit hook" > .git/hooks/pre-commit
$ chmod +x .git/hooks/pre-commit
```

我們基本上是在 hooks 資料夾中創建一個名為 `pre-commit` 的檔案，並賦予它執行權限。現在如果我們做 commit，應該會看到顯示該訊息。

```bash
$ echo "sample file" > sample.txt
$ git add sample.txt
$ git commit -m "adding sample file"
this is from pre commit hook     # <===== 來自 hook 執行的訊息
[master 9894e05] adding sample file
1 file changed, 1 insertion(+)
create mode 100644 sample.txt
```

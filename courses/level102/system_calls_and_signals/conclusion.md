# 結論

SRE 的主要目標之一是提升高規模系統的可靠性。為了達成這個目標，對系統內部運作的基本理解是必要的。

了解信號的運作方式非常重要，因為信號在程序的生命週期中扮演著重要角色。我們可以看到信號在多種程序操作中的使用：從建立程序到終止程序。對信號的認識尤其重要，當你在程式中處理信號時，如果預期會有觸發信號的事件，可以定義一個處理函式並告訴作業系統，在收到該種類型的信號時執行這個函式。

理解系統呼叫對 SRE 在除錯任何 Linux 程式時尤其有用。系統呼叫提供對作業系統內部功能的精確認識，並讓程式設計人員對 C 函式庫中在較低層實作系統呼叫的方式有深入理解。利用 *strace* 指令，可以輕鬆地除錯執行緩慢或當掉的程序。

# 延伸閱讀
<https://www.oreilly.com/library/view/understanding-the-linux/0596002130/ch01s06.html>

<https://jvns.ca/blog/2021/04/03/what-problems-do-people-solve-with-strace/>

<https://medium.com/@akhandmishra/important-system-calls-every-programmer-should-know-8884381ceadb>

<https://www.brendangregg.com/blog/2014-05-11/strace-wow-much-syscall.html>

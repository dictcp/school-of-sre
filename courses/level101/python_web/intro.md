# Python 與網路

## 先備知識

- 基本的 Python 語言理解。
- 基本的 Flask 框架熟悉度。

## 本課程可期望之內容

本課程分為兩個高層次的部分。第一部分假設你已熟悉 Python 語言的基礎操作與語法使用，我們將更深入探討 Python 語言本身。我們會把 Python 與你可能熟悉的其他程式語言（如 Java 和 C）做比較。我們也會探討 Python 物件的概念，並藉此探索 Python 的特性如裝飾器 (decorators)。

第二部分則圍繞於網路，並假設你熟悉 Flask 框架，我們將從 `socket` 模組開始，處理 HTTP 請求。這將幫助你了解像 Flask 這類框架內部如何運作。

此外，為了加入 SRE 的風味，我們將在理論上設計、開發並部署一個 URL 縮短應用。我們會強調作為該應用/服務的 SRE，整個過程中哪些部分更重要。

## 本課程不涵蓋內容

Python 內部機制的深入知識及進階 Python。

## 實驗環境設定

安裝最新版 Python。

## 課程內容

1. [Python 語言](https://linkedin.github.io/school-of-sre/level101/python_web/intro/#the-python-language)
      1. [一些 Python 概念](https://linkedin.github.io/school-of-sre/level101/python_web/python-concepts/)
      2. [Python 的陷阱](https://linkedin.github.io/school-of-sre/level101/python_web/python-concepts/#some-gotchas)
2. [Python 與網路](https://linkedin.github.io/school-of-sre/level101/python_web/python-web-flask/)
      1. [Socket](https://linkedin.github.io/school-of-sre/level101/python_web/python-web-flask/#sockets)
      2. [Flask](https://linkedin.github.io/school-of-sre/level101/python_web/python-web-flask/#flask)
3. [URL 縮短應用](https://linkedin.github.io/school-of-sre/level101/python_web/url-shorten-app/)
      1. [設計](https://linkedin.github.io/school-of-sre/level101/python_web/url-shorten-app/#design)
      2. [應用擴展](https://linkedin.github.io/school-of-sre/level101/python_web/sre-conclusion/#scaling-the-app)
      3. [應用監控](https://linkedin.github.io/school-of-sre/level101/python_web/sre-conclusion/#monitoring-strategy)

## Python 語言

假設你對 C/C++ 和 Java 有一些認識，我們嘗試在這兩種語言與 Python 的語境下討論以下問題。你可能聽過 C/C++ 是編譯型語言，而 Python 是直譯型語言。一般來說，編譯型語言我們會先編譯程式，然後執行可執行檔；而 Python 則是直接執行原始碼，如 `python hello_world.py`。而 Java 雖然是直譯語言，但還是先有編譯階段，再執行。所以，兩者有什麼真正差別？

### 編譯 vs 直譯

這聽起來可能有點怪：Python 在某種程度上也是編譯型語言！Python 內建編譯器！Java 明顯是這樣，我們用獨立命令編譯它，例如：`javac helloWorld.java` 會產生 `.class` 檔，也就是所謂的 _bytecode_。Python 與其非常相似。差別在於 Python 不需要獨立的編譯命令/二進位檔就能執行。

**那 Java 與 Python 差別在哪裡？**  
Java 的編譯器更嚴格且複雜。你應該知道 Java 是靜態型別語言，編譯器能在編譯階段驗證型別相關錯誤。而 Python 是 _動態_ 語言，型別直到程式執行時才知道。因此 Python 編譯器相對“簡單”或說較不嚴格。但執行 Python 程式時確實有編譯步驟。你可能見過 Python 的 `.pyc` 位元組碼檔。以下示範如何查看 Python 程式的位元組碼：

```bash
# 建立 Hello World
$ echo "print('hello world')" > hello_world.py

# 確認可以執行
$ python3 hello_world.py
hello world

# 查看該程式的位元組碼
$ python -m dis hello_world.py
 1           0 LOAD_NAME                0 (print)
             2 LOAD_CONST               0 ('hello world')
             4 CALL_FUNCTION            1
             6 POP_TOP
             8 LOAD_CONST               1 (None)
            10 RETURN_VALUE
```

關於 `dis` 模組請參考[官方說明](https://docs.python.org/3/library/dis.html)。

至於 C/C++，當然有編譯器。但輸出結果與 Java/Python 不同。C 編譯後會產生我們稱為 _機器碼_，而非 _位元組碼_。

### 執行程式

我們知道這三種語言皆包含編譯過程，只是編譯器差異和輸出內容不同。C/C++ 產出的是機器碼，可被作業系統直接讀取執行。當你執行程式時，作業系統知道怎麼執行它。**但位元組碼則不然。**

位元組碼是語言專有的。Python 有自己定義的位元組碼（可用 `dis` 模組查看），Java 也一樣。因此作業系統本身不懂怎麼執行它。為了執行這些位元組碼，我們有「虛擬機」（Virtual Machines，VM）。例如 JVM 或 Python VM（CPython、Jython）。這些所謂的虛擬機是能解讀位元組碼並在特定作業系統上執行的程式。Python 有多種 VM 可用。CPython 是用 C 實作的 Python 虛擬機，Jython 是 Java 平台的 Python VM 實作。**最終，它們應該能理解 Python 語法，編譯成位元組碼，並執行該位元組碼。** 你甚至可以用任意語言實作 Python VM！（且有人真的做了）

```
                                                              作業系統

                                                              +------------------------------------+
                                                              |                                    |
                                                              |                                    |
                                                              |                                    |
hello_world.py                    Python 位元組碼             |         Python VM 程式            |
                                                              |                                    |
+----------------+                +----------------+          |         +----------------+         |
|print(...       |   編譯         |LOAD_CONST...   |          |         |逐行讀取位元組碼|         |
|                +--------------->+                +------------------->|並執行         |         |
|                |                |                |          |         |                |         |
|                |                |                |          |         |                |         |
+----------------+                +----------------+          |         +----------------+         |
                                                              |                                    |
                                                              |                                    |
                                                              |                                    |
hello_world.c                     特定平台機器碼             |         新執行序                  |
                                                              |                                    |
+----------------+                +----------------+          |         +----------------+         |
|void main() {   |   編譯         | 二進位內容     |          |         | 二進位內容     |         |
|                +--------------->+                +------------------->|                |         |
|                |                |                |          |         |                |         |
|                |                |                |          |         |                |         |
+----------------+                +----------------+          |         +----------------+         |
                                                              |         （機器碼直接執行）         |
                                                              |                                    |
                                                              |                                    |
                                                              +------------------------------------+
```

上述圖解需注意兩點：

1. 一般執行 Python 程式時會啟動 Python VM 程式，該程式讀取原始碼，動態編譯成位元組碼並執行，編譯並非獨立步驟，圖中僅為說明。
2. C 產生的二進位檔不一定是直接執行檔（因各種二進位格式如 ELF），執行前作業系統還有其他複雜處理流程，我們不在此介紹，因其為作業系統層面工作。

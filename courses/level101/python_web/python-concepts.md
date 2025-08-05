# 一些 Python 概念

雖然你預期已經了解 Python 及其語法的基礎，但讓我們來討論一些基本概念，這將有助於你更好地理解 Python 語言。

**Python 的一切都是物件。**

這包括函數、列表、字典、類別、模組、正在執行的函數（函數定義的實例），一切。在 CPython 中，這意味著每個物件底層都有一個 `struct` 變數。

在 Python 當前的執行環境中，所有變數都儲存在一個 dict 中。這是一個字串到物件的映射。如果你在當前環境中定義了一個函數和一個浮點數變數，以下是它們如何被內部處理。

```python
>>> float_number=42.0
>>> def foo_func():
...     pass
...

# 注意變數名稱是字串，儲存在 dict 中
>>> locals()
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'float_number': 42.0, 'foo_func': <function foo_func at 0x1055847a0>}
```

## Python 函數

因為函數也是物件，我們可以看看函數擁有的屬性，如下所示：

```python
>>> def hello(name):
...     print(f"Hello, {name}!")
...
>>> dir(hello)
['__annotations__', '__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__',
'__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__get__', '__getattribute__', '__globals__', '__gt__',
'__hash__', '__init__', '__init_subclass__', '__kwdefaults__', '__le__', '__lt__', '__module__', '__name__',
'__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__',
'__subclasshook__']
```

雖然屬性很多，但我們來看看一些有趣的。

#### __globals__

這個屬性，顧名思義，包含了全域變數的引用。如果你想知道在函數作用域內有哪些全域變數，這可以告訴你。看看函數開始看到新變數在 globals 中的樣子。

```python
>>> hello.__globals__
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'hello': <function hello at 0x7fe4e82554c0>}

# 新增全域變數
>>> GLOBAL="g_val"
>>> hello.__globals__
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'hello': <function hello at 0x7fe4e82554c0>, 'GLOBAL': 'g_val'}
```

### __code__

這是一個有趣的屬性！因為 Python 的一切皆物件，包括位元碼。編譯後的 Python 位元碼是一個 Python 程式碼物件，可以透過 `__code__` 屬性存取。函數有一個相關聯的程式碼物件，攜帶一些有趣的資訊。

```python
# 函數定義所在檔案
# 這裡是 stdin，因為在直譯器中執行
>>> hello.__code__.co_filename
'<stdin>'

# 函數參數數量
>>> hello.__code__.co_argcount
1

# 區域變數名稱
>>> hello.__code__.co_varnames
('name',)

# 函數編譯後的位元碼
>>> hello.__code__.co_code
b't\x00d\x01|\x00\x9b\x00d\x02\x9d\x03\x83\x01\x01\x00d\x00S\x00'
```

還有更多程式碼物件的屬性可以用 `>>> dir(hello.__code__)` 查看。

## 裝飾器（Decorators）

跟函數相關的是，Python 有另一個叫做裝飾器的功能。讓我們看看它是怎麼運作的，並保持「一切皆物件」的觀念。

這是一個範例裝飾器：

```python
>>> def deco(func):
...     def inner():
...             print("before")
...             func()
...             print("after")
...     return inner
...
>>> @deco
... def hello_world():
...     print("hello world")
...
>>>
>>> hello_world()
before
hello world
after
```

這裡 `@deco` 語法用於裝飾 `hello_world` 函數。它本質上等同於：

```python
>>> def hello_world():
...     print("hello world")
...
>>> hello_world = deco(hello_world)
```

`deco` 函數內部發生什麼看起來可能有點複雜，我們嘗試拆解它。

1. 建立函數 `hello_world`
2. 將它傳入 `deco` 函數
3. `deco` 建立一個新的函數
      1. 新函數呼叫 `hello_world` 函數
      2. 並執行其他幾個動作
4. `deco` 傳回新建立的函數
5. `hello_world` 被取代成上述函數

我們利用圖示幫助理解：

```
       裝飾前             函數物件 (ID: 100)

       "hello_world"         +--------------------+
               +            |print("hello_world")|
               |            |                    |
               +----------> |                    |
                            |                    |
                            +--------------------+


       裝飾器的作用

       建立一個新函數 (ID: 101)
       +---------------------------------+
       |輸入參數：函數物件 ID: 100        |
       |                                 |
       |print("before")                  |
       |呼叫函數物件 ID: 100             |
       |print("after")                   |
       |                                 |
       +---------------------------------+
                                   ^
                                   |
       裝飾後                       |
                                   |
                                   |
       "hello_world" +--------------+
```

注意 `hello_world` 名稱指向一個新函數物件，但該函數物件知道原始函數的引用（ID）。

## 一些注意事項

- 雖然在 Python 中建立原型非常快速且有大量庫可用，但隨著程式碼基礎複雜度增加，型別錯誤變得更常見且難以處理。（對此有解決方案，如 Python 的型別註解。可參考 [mypy](http://mypy-lang.org/)。）
- 由於 Python 是動態型別語言，這代表所有型別都是在執行時決定。這使得 Python 執行速度比其他靜態型別語言慢很多。
- Python 有個東西叫做 [GIL](https://www.dabeaz.com/python/UnderstandingGIL.pdf)（全域解譯器鎖），這限制了多核心 CPU 做平行計算的能力。
- Python 做的一些怪異之處：[https://github.com/satwikkansal/wtfpython](https://github.com/satwikkansal/wtfpython)。

## With statement

在我們談到 `with` 語句時，就不得不先提到所謂的 `Context Manager` ，有人翻譯成上下文管理器，但我覺得稱為情境管理即可，簡單來講就是希望規範一個物件在進入與離開 `with` 語句所創造的情境範圍內時能夠自動進行一些動作，打個比方就是進入浴室洗澡要脫衣服，洗完澡離開浴室要穿衣服，在這個比喻中浴室洗澡就是情境(Context)，脫穿衣服就是進入與離開這個情境下會需要進行的動作。

又或者程式設計師經常開完檔之後忘記關檔，所以我們會希望在 `with` 語句中進行讀檔的處理，並且讓程式執行離開 `with` 語句的範圍時，自動進行關檔，以免除一些繁瑣的動作，避免類似忘記關檔的事情發生(詳見隨手養成 Python 好習慣－開檔、讀檔)。

那麼如何實作 Context Manager？
很簡單，只要在類別中實作 `__enter__()`, `__exit__()` 2 個方法即可。

同樣以洗澡為例撰寫以下範例：

```python
#! -*- coding: utf-8 -*-

class Shower(object):

    def put_on_clothes(self):
        print "put on clothes"

    def take_off_clothes(self):
        print "take off clothes"

    def __enter__(self):
        print "enter bathroom"
        self.take_off_clothes()
        return 'naked now.'

    def __exit__(self, exc_type, exc_value, traceback):
        self.put_on_clothes()
        print "exit bathroom"


if __name__ == '__main__':
    with Shower() as take_a_shower:
        print take_a_shower
```

上述程式執行結果：

```
enter bathroom
take off clothes
naked now
put on clothes
exit bathroom
```


`__enter__()` 方法就如同其名稱，沒有需要特別解釋的，主要也只是必須回傳一個值供 `with` 語句使用。

而 `__exit__(self, exc_type, exc_value, traceback)` ，則多了 3 個參數可供使用(例外類型、例外的值、以及 traceback，如果執行 with 的過程中沒有產生例外，則此 3 個參數都會是 None，若有產生例外，則會依照例外情況自動將這 3 個參數傳遞給 `__exit__()` 方法使用。

同樣以一個範例來看 `__exit__()` 處理例外時的情況，我們故意讓回傳的 “naked now” 字串去呼叫一個不存在的方法，以觸發例外：

```
class Shower(object):

    def put_on_clothes(self):
        print "put on clothes"

    def take_off_clothes(self):
        print "take off clothes"

    def __enter__(self):
        print "enter bathroom"
        self.take_off_clothes()
        return 'naked now'

    def __exit__(self, exc_type, exc_value, traceback):
        if not exc_type:
            self.put_on_clothes()
            print "exit bathroom"
        else:
            print "Exception", exc_type
            print "Exception Value", exc_value
            print "Traceback", traceback


if __name__ == '__main__':
    with Shower() as take_a_shower:
        take_a_shower.do_something()
```

上述程式執行結果如下：

```
enter bathroom
take off clothes
Exception type ‘exceptions.AttributeError’
Exception Value 'str' object has no attribute 'do_something'
Traceback traceback at 0x10c3f2a28
Traceback (most recent call last):
  File "withstatment.py", line 34, in <module>
    take_a_shower.do_something()
AttributeError: 'str' object has no attribute 'do_something'
```

所以談到目前為止，with 語句的功用其實就是依照以引述自官方文件的流程進行呼叫：

```
The context expression (the expression given in the with_item) is evaluated to obtain a context manager.

The context manager’s __exit__() is loaded for later use.
The context manager’s __enter__() method is invoked.
If a target was included in the with statement, the return value from __enter__() is assigned to it.
The with statement guarantees that if the __enter__() method returns without an error, then __exit__() will always be called. Thus, if an error occurs during the assignment to the target list, it will be treated the same as an error occurring within the suite would be. See step 6 below.
The suite is executed.
The context manager’s __exit__() method is invoked. If an exception caused the suite to be exited, its type, value, and traceback are passed as arguments to __exit__(). Otherwise, three None arguments are supplied.
```

所以 with 語句就是利用 Context Manager 所帶來的強大功用，讓每一位 Python 程式設計師能夠針對自己撰寫的物件搭配進行情境的管理。

## 後記

python 還有提供一個更簡單的 decorator 方便實作 context manager - [contextlib](https://docs.python.org/2/library/contextlib.html)

參考資料：
https://docs.python.org/2/reference/compound_stmts.html#with
https://docs.python.org/2/reference/datamodel.html#context-managers

# String

在 Python 用 `"` , `'` 或者 `"""` , `'''` 包起來的文字，就是字串，例如：

```

str1 = "Hello World"


str2 = '''
    Hello World
'''

```

上述 2 者的差別在於， `str2` 有隱含 2 個換行符號及 1 個 tab 在裡面， `str2` 其實就等於 `str3` ：

```

str3 = "\n\tHello World\n"

```

## Types of string in Python 2.7

在 Python 2.7 string 分為 2 種類型(其實頗讓人感到麻煩，因此在進行 string 操作時，要注意字串的類型是否一致)：

1. unicode (Python 3 之後，字串預設都是以 unicode 儲存)

2. str 或稱 byte stream

unicode 字串就是一般用 `u` 作爲 prefix 的字串，而 str 則沒有 `u` 作為 prefix 。

```
>>> print type(u'Hello World')
<type 'unicode'>

>>> print type('Hello World')
<type 'str'>
```

這 2 種類型的字串可以互相轉換：

```
unicode -> encode -> str

str -> decode -> unicode
```

例如以下範例實際操作就會發現有沒有 `u` 的小細節：

```
>>> u'Hello World'.encode('utf-8')
'Hello World'

>>> 'Hello World'.decode('utf-8')
u'Hello World'
```

上述的 `.encode('utf-8')` 與 `.decode('utf-8')` 指的就是要用何種編碼來轉換字串，其實就是下圖的過程：

```
unicode -> encode -> utf-8 str

utf-8 str -> decode -> unicode
```

而 `utf-8` 的編碼宣告通常都會被我們放在程式的第一行 `# -*- coding: utf-8 -*-` ，這是為了告訴 python interpreter 要用 utf-8 作為 str 的預設編碼。

因此，如果不加上 `# -*- coding: utf-8 -*-` 的話，遇到中文字或者其他英文字之外的字串時就會有下列的錯誤出現：

```
SyntaxError: Non-ASCII character '\xe4' in file chinese.py on line 1, but no encoding declared; see http://www.python.org/peps/pep-0263.html for details
```

這就是為何我們習慣加上 `# -*- coding: utf-8 -*-` 的原因。


如果想要知道系統預設的 encoding 可以用 `sys` 模組的 `getdefaultencoding()` 來取得：

```
>>> import sys
>>> sys.getdefaultencoding()
```

如果要改變系統預設編碼：

```
>>> import sys
>>> reload(sys)  # required, 因為 python initialize 時, setdefaultencoding 會被 site.py 刪除，所以需要 reload
>>> sys.setdefaultencoding('utf-8')
```


## LENGTH

Python 會因為 2 種不同的字串類型而有不同的長度計算方式，例如：

```
>>> print len(u'中文')
2

>>> print len('中文')
6

>>> print len(u'Hello')
5

>>> print len('Hello')
5
```

這是因為 unicode 裡 1 個字的長度就是 1 ，但是在 str 中是以 byte 數進行計算，剛好 `utf-8` 的中文字必須用 3 個 byte 儲存，所以計算出來的長度就總共是 6 。

ref: https://zh.wikipedia.org/wiki/UTF-8

## 好用的 unitities

```
def is_unicode(s):
    return True if isinstance(s, unicode) else False


def is_str(s):
    return True if isinstance(s, str) else False


def ensure_unicode(s, encoding='utf-8'):
    """
    >>> ensure_unicode('HelloWorld')
    u'HelloWorld'
    """

    if not is_unicode(s):
        return s.decode(encoding)

    return s


def ensure_str(s, encoding='utf-8'):

    if not is_str(s):
        return s.encode(encoding)

    return s

```

## Format

很多時候，

## codecs

主要用途：處理字串＆檔案編碼轉換(encode & decode)

## example

reading utf-8 files and convert to unicode

```python
import codecs

file_obj = codecs.open('utf8file.txt', 'r', 'utf-8')
unicode_content = file_obj.read()
```

## byte-order marker

不同的硬體架構會有不同的位元組順序(byte ordering)，對於像是 UTF-16, UTF-32 這種 multi byte encodings 就會有 byte ordering 的問題，因為我們沒辦法總是知道資料是用什麼位元組順序，所以可以在資料一開始就加上幾個 byte 來說明接下來的資料是用什麼 byte ordering, codecs 裡的 byte-order marker (BOM) 就是在做這件事情。

例如幫檔案加上 UTF-8 BOM:

```python
import codecs
file_obj = file('utf8file.txt', 'w')
file_obj.write(codecs.BOM_UTF8)
file_obj.write(u'中文'.encode('utf-8'))
file_obj.close()
```

## Standard Input/output streams

如果是在 Unix-like 的系統上試圖執行 python 列印 unicode string 或者將 standard output 丟給 pipeline 處理時，有時候會遇到 `UnicodeEncodeError` ，這是由於沒有為 `sys.stdout` 設定編碼的緣故。

例如以下程式正常執行不會有問題，但是加上 pipeline 之後就會有 `UnicodeEncodeError`

```python
# -*- coding: utf-8 -*-
print u'中文'
```

```
$ python print_to_stdout.py
中文
$ python print_to_stdout.py | cat
Traceback (most recent call last):
  File "print_to_stdout.py", line 2, in <module>
    print u'中文'
UnicodeEncodeError 中文: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```

為 `sys.stdout` 明確設定編碼可以使用 `codecs` 的 `getwriter`

```python

import codecs
import sys

encoding = 'UTF-8'

# 可以使用以下程式自動偵測編碼
# import locale
# locale.setlocale(locale.LC_ALL, '')
# lang, encoding = locale.getdefaultlocale()

utf8_stdout = codecs.getwriter(encoding)(sys.stdout)
utf8_stdout.write(u'中文') # 輸出到 stdout

sys.stdout = utf8_stdout # 取代原本的 stdout, 改用 codecs 的 writer 來輸出
print u'測試'
```

如此一來就會正常運作了

```
$ python print_to_stdout.py | cat
中文
測試
```



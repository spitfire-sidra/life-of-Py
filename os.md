
## os

列出資料夾底下的檔案，不包含資料夾

```
from os import listdir
from os.path import isfile
from os.path import join

mypath = '/home'

files = [f for f in listdir(mypath) if isfile(join(mypath,f))]
```

走訪所有的資料夾及檔案

walk 會回傳一個 generator ，每次都會回傳一個 3-tuple

(資料夾路徑, 資料夾底下的所有資料夾, 資料夾底下的所有檔案)

```
for (dirpath, dirnames, filenames) in walk(mypath):
    print dirpath
    print dirnames
    print filenames
```

預設的走訪方式是 Topdown ，如果要 bottom up 走訪，可以設定 `walk(mypath, topdown=False)`

但是 `topdown=False` 的效率比較不好，因為要先走訪完 dirnames 才能夠向上走訪 dirpath 。

此外， listdir 遇到 error 時是直接忽略，而 walk 則可以另外指定 `onerror` 參數來處理錯誤， `onerror` 必須是一個 function ，只接受一個參數，這個參數永遠都會是 `OSError` instance ，你可以在這個 function 中決定是否要繼續走訪或者進行紀錄錯誤等動作。

```
By default, errors from the listdir() call are ignored. If optional argument onerror is specified, it should be a function; it will be called with one argument, an OSError instance. It can report the error to continue with the walk, or raise the exception to abort the walk. Note that the filename is available as the filename attribute of the exception object.
```

例如：

```
def walk_onerror(exception):
    print exception.filename

for (dirpath, dirnames, filenames) in walk(mypath, onerror=walk_onerror):
    print dirpath, dirnames, filenames
```


## glob

** glob 是一種被 *nix 使用來做 pathnames matching 的規則，用途有點類似正規表示式，但沒有那麼完備

如果是要尋找含特定關鍵字或是固定命名模式的檔案，可以使用 glob 模組。

例如，尋找所有數字命名且副檔名是 `.mp3` 的檔案，可以表示成 `[0-9]*.mp3`：

```
>>> import glob
>>> glob.glob('./[0-9]*.mp3')
['./12.mp3', './1.mp3']
```

** `./` 開頭的字串表示當前資料夾

那如果資料夾檔案很多的話，建議可以使用 `iglob` ，減少記憶體的用量，因為 iglob 是回傳一個 iterator 。

```
>>> import glob
>>> glob.iglob('./[0-9]*.mp3')
<generator object iglob at 0x104c6bf00>
```

而 glob 其實是靠 `os.listdir()` 跟 `fnmatch.fnmatch()` 來實作的。

`fnmatch` 指的就是 `Unix filename pattern matching`

上述的 glob 範例可以改寫成：

```
import fnmatch
import os

for file in os.listdir('.'):
    if fnmatch.fnmatch(file, '[0-9]*.mp3'):
        print file
```

`fnmatch` 有個有趣的 method 叫做 `translate` ，可以把 glob pattern 轉成正規表示式，

例如：

```
>>> import fnmatch, re
>>> fnmatch.translate('[0-9]*.mp3')
'[0-9].*\\.mp3\\Z(?ms)'
```

## shutil

對於需要做系統檔案相關的操作，可以看看 shutil 模組，裡面已經定義好不少方法供我們使用。

例如刪除資料夾：

```
>>> import shutil
>>> shutil.rmtree('/folder_name')
```

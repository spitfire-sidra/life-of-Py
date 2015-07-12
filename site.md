# site

site module 會在 python 初始化時自動 import 的 module 。

所以如果要針對 python 進行一些 hook 的操作，可以透過 site package 達成。

要使用這個功能之前，可以先用以下程式碼查詢 site packages 的路徑：

```
python -m site --user-site
/Users/spitfire/Library/Python/2.7/lib/python/site-packages
```

接著，可以在上述路徑新增 `sitecustomize.py` 或 `usercustomize.py` 。

# sitecustomize & usercustomize

sitecustomize & usercustomize 是 2 個 python 所提供用來在初始化 python interpreter 時進行一些設定的方式。

sitecustomize 通常是 administrator 設定的 global configurations ，通常修改需要權限。

而 usercustomize 則是只會影響到當前使用者的 configuration ，會在 sitecustomize 執行完後接著執行。

接下來，我們在 `/Users/spitfire/Library/Python/2.7/lib/python/site-packages` 路徑下可以新增一個檔案 `usercustomize.py` ：

裡面只寫一行程式：

```
print 'Loading usercustomize.py'
```

接著執行 `python` 應該就會發現多一行 `Loading usercustomize.py` ：

```
$ python
Loading usercustomize.py
Python 2.7.6 (default, Sep  9 2014, 15:04:36)
[GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.39)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

這就是 site 的用法。


目前常見的用法是用來修改 `default encoding` ，例如：

```
import sys
sys.setdefaultencoding('utf-8')
```

或者修改 module 載入路徑：

```
import sys
sys.path.insert(
    0,
    os.path.join(os.environ['HOME'],
    'whatever/you/want')
)
```

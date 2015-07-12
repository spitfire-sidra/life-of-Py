# site

site module 會在 python 初始化時自動 import 的 module 。

所以如果要針對 python 進行一些 hook 的操作，可以透過 site package 達成。


查詢 site packages 的路徑：

```
python -m site --user-site
/Users/spitfire/Library/Python/2.7/lib/python/site-packages
```

然後在該路徑下可以新增一個檔案 `usercustomize.py`

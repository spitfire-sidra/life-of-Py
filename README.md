# 少年 Py 的奇幻漂流

## 前言

在「學徒模式:優秀軟體開發者的養成之路」這本書中，提到了不少個成為優秀軟體開發者所必須實踐的幾個原則，本文是基於「記錄個人所學」、「打造拋棄式玩具」、「分享所學」這 3 個原則而寫成的。

如同「少年 PI 的奇幻漂流」中的 3 個故事，本文也以 3 個主要過程來陳述利用 GitHub, Travis CI, Coveralls.io, PyPi 打造 Python 套件的過程與心得，希望能夠幫助更多人得到成長，不過編者也不是相當有經驗的人，如果有誤解，也請多多指教。

此 Python 套件名為 DateRanger，主要是寫來計算常用的商用日期區間。目前原始碼放在 GitHub 上，同時也可以透過 PyPi 安裝，如果有任何問題或者建議也歡迎在 GitHub 與編者反映。

## 開發的第一步 - 檢視你的需求＆打造原型

在編者的日常工作中，經常需要利用當前日期往回推算日期區間，以產生對應的數據報表，甚至一些監控的系統也需要類似的日期區間去產生對應的監控數據，每次都去翻月曆查幾號到幾號實在是相當的重複自己(repeat myself)，所以編者決定打造一個小小的拋棄式玩具，可以自動推算日期區間，以讓自己達到偷懶的目的！

所以編者檢視了自己的需求，發覺自己可能需要類似以下的功能來幫助自己：

```
>>> date_range = DateRanger()
>>> yesterday = date_range.prev_day(n=1)
# expect datetime.date(2015, 1, 1)
>>> last_week = date_range.prev_week(n=1)
# datetime.date(2014, 12, 28) to datetime.date(2015, 1, 3)
>>> last_month = date_range.prev_month(n=1)
...(略)...
```

因此，編者就開始打造了一些很簡略的原型試著驗證自己的想法，例如：

```
class DateRanger(object):

    def __init__(self, base_date=None):
        self.base_date = base_date or date.today()
    
    def pre_day(self, days=1):
        pass

    def pre_week(self, weeks=1):
        pass
...(略)...
```

接下來，就搭配 Git 版本控制與 GitHub 來慢慢將原型發展成比較有規劃的樣子。而 Git 與 GitHub 的教學在網路上也已經多不勝數，因此就不再贅述。

再來，編者的第二個需求是希望能夠將此一套件開放原始碼，讓每個人都能夠受惠。

所以最主要的部分就是必須將原本編者隨性的資料夾結構改成 pip 所規範的結構，以方便打包(packaging)。

基本的結構如下：

```
YOUR_PACAKGE_NAME/
├── YOUR_PACAKGE_NAME/
│   ├── __init__.py
│   └── test/
│        └── __init__.py
├── LICENSE.txt
├── MANIFEST.in
├── README.rst
├── setup.cfg
└── setup.py
```

檔案與資料夾用途說明：

* `LICENSE.txt` 開放原始碼授權條款
* `README.rst` 文件說明(必須以 reStructuredText 語法撰寫，目前 PyPi 不支援 Markdown 語法)
* `setup.py` 寫明套件的基本資訊與簡介，例如作者聯絡資訊(請不要寄垃圾信給我XD)，以及需要被打包的模組或者模組相依性
* `setup.cfg` 寫明一些setup相關設定或者metadata
* `MANIFEST.in` 除了模組之外，額外需要被打包的檔案，例如 `README.rst` 就需要放在這裡

更多的說明與教學詳見文件： <https://python-packaging-user-guide.readthedocs.org/en/latest/>

### 小結

這就是少年 Py 的第 1 個故事。看起來似乎很容易，但寫程式的過程也來回修改了很多次，這一切其實是肇因於編者並沒有定義比較清楚的規格，例如到底一周的結束是結束在週六好呢？還是週日？這樣問題就會影響一些方法實作，與後續的測試設計與實作。

這個過程還是告訴我們，除了原型之外，清楚的規格也是很重要的，才不會讓我們花太多時間在對付因為沒有定義規格而造成的實作上的模凌兩可。

“原型讓你看見可能性，規格為你消除不確定性。”

再來是文件還是需要先花點時間閱讀的，例如：筆者在測試利用 `pip` 安裝套件時是不是正常時就遇到了這樣的問題：

```
$ pip install DateRanger
Downloading/unpacking DateRanger
  Running setup.py egg_info for package DateRanger
    Traceback (most recent call last):
      File "<string>", line 14, in <module>
      File "/home/.../build/DateRanger/setup.py", line 15, in <module>
        long_description=open('README.rst').read(),
    IOError: [Errno 2] No such file or directory: 'README.rst'
    Complete output from command python setup.py egg_info:
    Traceback (most recent call last):

  File "<string>", line 14, in <module>

  File "/home/.../build/DateRanger/setup.py", line 15, in <module>

    long_description=open('README.rst').read(),

IOError: [Errno 2] No such file or directory: 'README.rst'
```

這時候就馬上知道 `README.rst` 忘了被打包進去，因此透過 `pip` 安裝時會發生這樣的問題，所以就知道要在 `MANIFEST.in` 中寫上 `include README.rst`。

“閱讀文件不是為了全部精通，而是遇到問題時讓我們知道可能的問題在哪裡以及如何解決。”

這就是第 1 個少年 Py 的小結。

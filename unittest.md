## unittest part1

本篇談談 Python 的 unittest 模組。顧名思義，unittest 就是專門做單元測試的模組。

從 Python 2.1 開始，Python 就增加了 unittest 單元測試模組，unittest 主要提供一個單元測試的框架(Framework)，讓每一位利用 Python 撰寫程式的人都能輕鬆的對程式進行測試。

本篇以介紹單元測試為主。如果各位是要針對整體程式流程及功能進行測試，那麼推薦各位可以利用 Google 搜尋相關整合測試(Integration testing)的教學文章。

本篇將以 Python 2.7 版本進行解說與範例的撰寫。如果各位偏好使用 Python 3 的話，也可以用 Python 2to3 將範例轉為 Python 3 版本試試看。

使用 Python unittest 的方法很簡單，只要簡單 import 即可。如下所示：

```
import unittest
```

使用 unittest 前的不可不知

接下來，在以實際的範例進行解說前，先必須對unittest模組中4個重要的名詞有所了解，這4個重要的名詞是Test fixture、Test case、Test suite、Test runner，以下個別進行解說。

- Test fixture

    Test fixture 指的是進行一項測試前所需進行的準備工作，例如要測試資料庫的讀寫時，所需要的 Test fixture 就有可能是建立資料庫連線、建立測試用的資料表等。

- Test case

    Test case 是最小的測試單位，每一個 Test case 只會用來測試一項很小的程式邏輯(或者函式)是否如預期般運作正常。

- Test suite

    Test suite 則是多個 Test case 或者 Test suite 的集合，專門用來測試需要多個單元測試同時進行的測試項目，例如測試音樂播放軟對於各種格式音訊檔案的支援度，就可能需要 mp3、midi、wav 等多種音訊格式的 Test case 一起進行測試，此種由多個 Test case 組成的測試，就可稱為 Test suite。

- Test runner

    在 unittest 模組中，Test runner 負責的工作是測試的執行與測試結果的呈現。例如執行測試之後，並把結果顯示給測試者。

接下來實際撰寫一個 Test case 來認識 Test fixture、Test case 及 Test runner，詳見範例 1。

範例 1：

```
import unittest

def compare_string(s1, s2):
    if s1 == s2:
        return True
    else:
        return False

class MyFirstTest(unittest.TestCase):

    def setUp(self):
        self.default_greeting = u"Hello"

    def test_compare_string(self):
        test_greeting = u"HellO"
        self.assertFalse(compare_string(self.default_greeting, test_greeting))

    def test_compare_hex_string(self):
        hex_greeting = b"\x48\x65\x6c\x6c\x6f"
        self.assertTrue(compare_string(self.default_greeting, hex_greeting))

if __name__ == '__main__':
    tests = unittest.TestLoader().loadTestsFromTestCase(MyFirstTest)
    unittest.TextTestRunner(verbosity=2).run(tests)
```

範例 1 執行結果如下：

```
test_compare_hex_string (__main__.MyFirstTest) ... ok
test_compare_string (__main__.MyFirstTest) ... ok

----------------------------------------------------------------------
Ran 2 tests in 0.002s

OK
```

上述的範例 1 ，compare 函式就是我們要測試的目標。

為了進行測試，我們利用 Python 提供的 unittest 模組建立了 MyFirstTest 類別，此類別繼承 unittest.TestCase 類別，這是由於所有的 Test Case 都需要繼承自 unittest.TestCase 類別才能夠自動被 Test runner 載入並執行。

接著，把焦點轉移至 setUp 方法，setUp 方法就是上述 4 個重要觀念中的 Test fixture，在這個方法中，我們可以把一些變數初始化以供測試用。在範例1中，我們建立了 self.default_greeting 屬性做為測試 compare 函式的比較變數之用。

剩下的 test_compare_string 與 test_compare_hex_string 則是 Test case，也就是測試的最小單位，compare 必須要通過這 2 個 Test case 才會被認為有通過測試。Python 的 unittest 模組規定所有的 Test case 方法的命名都必須以 test 開頭(當然，這可以變更)，因此 範例1 中的 Test case 也都配合以 test 做為開頭，如果沒有以 test 開頭的話，就不會被認為是 Test case 而不會被執行！所以必須特別注意！而且所有以 test 開頭的測試方法並無法設定執行的順序，也就是說我們無法在類別中指定先執行 test_compare_hex_string 後再執行 test_compare_string，一切由 unittest 模組決定執行順序(預設按照字元順序)。

由於單元測試的目的在於測試每一個最小單元的程式邏輯(可以是一個函式)是否正確，因此如果我們所規劃的測試需要被指定先後順序，才能夠進行測試時，就代表被測式程式以及我們所撰寫的程式可以被拆解成更小的單元測試進行，當我們能夠把測試拆解成不需要指定先後次序，而且每一個單元測試都能夠獨立運作時，就是一個良好的Test case，也就能夠達成我們進行單元測試的目的了。

此外，在範例 1 的 2 個 Test case 中，我們也使用了繼承自 unittest 模組的 assert 相關方法，用來評斷測試的結果是否如同我們預期的結果，如果有任何不在預期的結果發生，就代表測試沒有通過。

相關的 assert 方法可以至 Python 的官方手冊查看。

最後範例 1 使用了 unittest.TestLoader().loadTestsFromTestCase() 函式，載入了我們所撰寫的 Test case，也就是 MyFirstTest 類別，然後使用 unittest.TextTestRunner(verbosity=2).run(tests)，執行我們所撰寫的測試，而 TextTestRunner 類別的角色就是上述提及的 Test runner。

目前為止，我們已經透過範例 1 認識了 Test fixture、Test runner 以及 Test runner，在進一步學習 unittest 之前，先小結一下目前所學到的關於使用 unittest 的須知。
所有的測試類別都必須繼承 unittest.TestCase
測試類別中的單元測試方法必須以 test 開頭作為命名
測試類別中的單元測試方法無法被指定順序
範例1中並未介紹如何建立 Test suite 。當程式隨著功能的增加而越來越龐大時，測試的項目就會理所當然隨之增加，因此測試類別就有可能越來越多，最後我們就需要針對一些相關的測試進行分類的管理。此時，我們就需要 Test suite 來進行管理，Test suite 的範例如下所示(範例 2 )：

```
import unittest

def compare(v1, v2):
    if v1 == v2:
        return True
    else:
        return False

class MyFirstTest(unittest.TestCase):

    def setUp(self):
        self.default_greeting = u"Hello"

    def test_compare_string(self):
        test_greeting = u"HellO"
        self.assertFalse(compare(self.default_greeting, test_greeting))

    def test_compare_hex_string(self):
        hex_greeting = b"\x48\x65\x6c\x6c\x6f"
        self.assertTrue(compare(self.default_greeting, hex_greeting))

class MySecondTest(unittest.TestCase):
    def test_compare_int_bool(self):
        v1 = 1
        v2 = True
        self.assertTrue(compare(v1, v2))

    def test_compare_type(self):
        v1 = type(1)
        v2 = type(True)
        self.assertFalse(compare(v1, v2))

if __name__ == '__main__':
    test1 = unittest.TestLoader().loadTestsFromTestCase(MyFirstTest)
    test2 = unittest.TestLoader().loadTestsFromTestCase(MySecondTest)

    suite = unittest.TestSuite()
    suite.addTests(test1)
    suite.addTests(test2)

    unittest.TextTestRunner(verbosity=2).run(suite)
```

```
範例 2 執行結果如下所示：
test_compare_hex_string (__main__.MyFirstTest) ... ok
test_compare_string (__main__.MyFirstTest) ... ok
test_compare_int_bool (__main__.MySecondTest) ... ok
test_compare_type (__main__.MySecondTest) ... ok

----------------------------------------------------------------------
Ran 4 tests in 0.001s

OK
```

在範例 2 中以 unittest.TestSuite() 建立一個 Test suite，並以 addTests 方法把 MyFirstTest、MySecondTest 加到 Test suite 中，最後以 TextTestRunner 執行此一 Test suite 中的所有單元測試方法。

認識了 unittest 模組中的 4 個基本概念後，我們就能夠將一些常用的測試項目模組化，並且建立不同的 Test suite 進行管理，除了能夠減少測試成本外，也能夠增加測試效率，避免浪費太多的時間與精力在撰寫功用類似的測試上，也能夠使得測試的程式具有可重覆使用性。

不過截至目前為止仍有些無法盡善盡美之處，接下來本文會在 Part 2 針對 unittest 模組做更進一步的介紹。

## unittest part2

繼 Python unittest 模組 Part 1 之後，本文繼續介紹幾個重要的方法。

### 測試也需要善後工作－ tearDown

測試過程難免會需要一些 Test fixture，例如產生設定檔、連結資料庫、產生檔案等等，而這些 Test fixture 也可能需要在測試完成後進行善後與清除，此時可以選擇覆寫 unittest 模組的 tearDown 方法， tearDown 會在每一個測試方法執行完之後被呼叫( setUp 則相反)，因此可以把測試的善後與清除的工作實作在 tearDown 方法中(不過也有人將測試過程的錯誤訊息在此方法中一併顯示)。詳情可以直接參照以下範例 3。

記得，unittest 模組每一次執行測試方法的順序是 setUp > test* 開頭的測試方法 > tearDown

範例 3：

```
import unittest

def compare(v1, v2):
    if v1 == v2:
        return True
    else:
        return False

class MyTest(unittest.TestCase):
    def setUp(self):
        print "setUp!"
        self.default_greeting = u"Hello"
        self.test_greeting = u"HellO"
        self.hex_greeting = b"\x48\x65\x6c\x6c\x6f"
        self.v1, self.v2 = (1, True)

    def test_compare_string(self):
        assert compare(self.default_greeting, self.test_greeting) == False

    def test_compare_hex_string(self):
        assert compare(self.default_greeting, self.hex_greeting) == True

    def test_compare_int_bool(self):
        assert compare(self.v1, self.v2) == True

    def tearDown(self):
        print "tearDown!"

if __name__ == '__main__':
    unittest.main()
```

範例 3 執行結果：

```
setUp!
tearDown!
.setUp!
tearDown!
.setUp!
tearDown!
.
----------------------------------------------------------------------
Ran 3 tests in 0.000s

OK
```

### Test Discover

範例 2 中提及利用 Test suite 對測試進行分類管理，除了能夠將性質相似的 Test case 放在同一個類別或檔案之外，也可以將這些測試程式碼以資料夾進行分類管理，再搭配 unittest 的 test discover 功能，就能夠執行指定的資料夾內的所有測試程式。

首先假設我們建立了一個專案資料夾為 MyProject ，接著在這資料夾內建立了一個 Tests 資料夾，Tests 資料夾內有兩個 Test case 以及一個 __init__.py，資料夾的結構如下所示：

```
MyProject/
    Tests/
        Case1_tests.py
        Case2_tests.py
        __init__.py
```

接下來，我們就能夠使用以下指令讓 Python 自動載入 Test case 並且執行：

```
python -m unittest discover -t /path/to/MyProject -s ./Tests/ -p '*_tests.py'
```

上述指令的意思代表在 MyProject 資料夾載入 Tests 資料夾內所有檔案以 _tests.py 結尾的 Test case 並執行測試。

這就是 unittest 的 Test Discover 功能！

### 舊有的 Test case 回收再利用

有些時候可能會遇到舊有的專案已經有一些既有的測試函式，在這種情況下我們也不一定非得要捨棄這些測式函式不用(當然，有時間的話，物件化會是 Python 官方比較推薦的方式)， Python 針對這種情況也設計了 FunctionTestCase 類別，讓我們將這些既有的測試函式轉換為 Test case 物件，範例 4 即是將舊有的測試函式轉換為 Test case 物件的例子，在範例中可以看到可以利用 FunctionTestCase 類別將各個舊有的測試函數包裝成 Test case 後進行測試。

範例 4：

```
import unittest

def compare(v1, v2):
    if v1 == v2:
        return True
    else:
        return False

def setUp():
    global v1, v2
    global default_greeting
    global test_greeting
    global hex_greeting
    default_greeting = u"Hello"
    test_greeting = u"HellO"
    hex_greeting = b"\x48\x65\x6c\x6c\x6f"
    v1, v2 = (1, True)

def test_compare_string():
    assert compare(default_greeting, test_greeting) == False

def test_compare_hex_string():
    assert compare(default_greeting, hex_greeting) == True

def test_compare_int_bool():
    assert compare(v1, v2) == True

def test_compare_type():
    assert compare(v1, v2) == False

def tearDown():
    default_greeting = None
    test_greeting = None
    hex_greeting = None
    v1, v2 = (None, None)

if __name__ == '__main__':
    testcases = (test_compare_string, test_compare_hex_string, test_compare_int_bool, test_compare_type)
    for test in testcases:
        testcase = unittest.FunctionTestCase(test, setUp=setUp, tearDown=tearDown)
        unittest.TextTestRunner(verbosity=2).run(testcase)
```

範例 4 執行結果：

```
unittest.case.FunctionTestCase (test_compare_string) ... ok

----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
unittest.case.FunctionTestCase (test_compare_hex_string) ... ok

----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
unittest.case.FunctionTestCase (test_compare_int_bool) ... ok

----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
unittest.case.FunctionTestCase (test_compare_type) ... FAIL

======================================================================
FAIL: unittest.case.FunctionTestCase (test_compare_type)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "ut3.py", line 29, in test_compare_type
    assert compare(v1, v2) == False
AssertionError

----------------------------------------------------------------------
Ran 1 test in 0.002s

FAILED (failures=1)
```

雖然範例 4 的用法看似方便，但事實上並不是一個被 Python 官方推薦的方法，應盡量避免使用 FunctionTestCase。

### Skipping tests & Expected failures

Python 2.7之後，在 unittest 模組也增加了 Skipping tests 跟 Expected failures 的功能。

Skipping tests 可以針對測試方法的執行進行條件的設定，例如滿足特定條件就略過測試。而Expected failures 則可以使用在某些預期必定會無法通過的測試方法上，使得無法通過的測試方法不會被視為沒有通過此一測試方法(因為預期測試方法根本不會被通過，所以沒有通過測試應被視為是通過測試才行，例如測試故意打錯帳號密碼，要是通過就有鬼了)。

範例 5 展示了 Skipping tests 跟 Expected failures 的方法：

```
import sys
import unittest

mylib_version  = (1, 3)


class MyTestCase(unittest.TestCase):

    @unittest.skip("demonstrating skipping")
    def test_nothing(self):
        self.fail("shouldn't happen")

    @unittest.skipIf(mylib_version < (1, 3),
                     "not supported in this library version")
    def test_format(self):
        # Tests that work for only a certain version of the library.
        pass

    @unittest.skipUnless(sys.platform.startswith("win"), "requires Windows")
    def test_windows_support(self):
        # windows specific testing code
        pass

@unittest.skipIf(mylib_version > (1, 2),  "not supported in this library version")
class MySkippedClass(unittest.TestCase):
    def test_nothing(self):
        pass

class ExpectedFailureTestCase(unittest.TestCase):
    @unittest.expectedFailure
    def failure_test(self):
        self.assertEqual(1, 0, "broken")

if __name__ == '__main__':
    unittest.main()
範例 5 執行結果(s 代表 skip)：
s.ss
----------------------------------------------------------------------
Ran 4 tests in 0.001s

OK (skipped=3)
```

範例 5 的重點在於對於想要設定 Skipping 或者 Expected failure 的測試方法可以使用 Python 裝飾方法(decorator)的方式進行設定，相當簡單易懂。

最後我們在範例 6 中展示如何利用 unittest.skip 客製化自己的 Skipping 的裝飾方法。

範例 6：

```
import sys
import unittest

class MyClass(object):
    do_test = True

def skipUnlessHasattr(obj, attr):
    if hasattr(obj, attr):
        return lambda func: func
    return unittest.skip("{!r} doesn't have {!r}".format(obj, attr))

@skipUnlessHasattr(MyClass, "test")
class MySkippedClass(unittest.TestCase):
    def test_nothing(self):
        pass

if __name__ == '__main__':
    unittest.main()
```

範例 6 的 skipUnlessHasattr 函式利用回傳 unittest.skip 來達成客製化 Skipping 裝飾方法的效果。

以上就是本篇 unittest 模組的大致教學與使用範例，在學會使用 unittest 之後，各位也可以學著開始利用一些以 unittest 模組為基礎而開發的測試套件(例如 nose)，相信一定可以駕輕就熟。

## Coverage

Coverage 簡單來說是我們用來了解多少程式有被測試到的一種衡量方式，換句話說，就是看我們所寫的測試到底測過受測程式的哪些部份。

這邊觀念上要注意的是， Coverage 無法告訴我們程式品質到底好不好。

```
$ pip install coverage
```

產生 coverage 報告

```
$ coverage -x Case1_tests.py
```

```
$ coverage -rm
```

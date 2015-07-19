# list

Python 的 list 其實很像 PHP 裡的 array ，可以儲存各種資料，也不須事先宣告長度。

但是又更加方便，除了實作有 `append` , `pop` , `sort` 等方法之外，也支援 `slice` 的操作。

## Slice

```
a[start:end] # items start through end-1
a[start:]    # items start through the rest of the array
a[:end]      # items from the beginning through end-1
a[:]         # a copy of the whole array
```

There is also the step value, which can be used with any of the above:

a[start:end:step] # start through not past end, by step
The key point to remember is that the :end value represents the first value that is not in the selected slice.
So, the difference beween end and start is the number of elements selected (if step is 1, the default).

The other feature is that start or end may be a negative number, which means it counts from the end of the array instead of the beginning. So:
```
a[-1]    # last item in the array
a[-2:]   # last two items in the array
a[:-2]   # everything except the last two items
```

ref: http://stackoverflow.com/questions/509211/explain-pythons-slice-notation

### Shallow copy

python slice 其實是回傳新的 list ，也就是多一塊記憶體配置，但是裡面的 element 指標是指向原本 list 裡的 element，這被 python 稱為 shallow copy 。

All slice operations return a new list containing the requested elements.

This means that the following slice returns a new (shallow) copy of the list:

```
>>> a = [1, 2, 3]
>>> b = a[:]
>>> id(a)
4383693424
>>> id(b)
4383694360
```

ref: https://docs.python.org/2/library/copy.html


## Concatenate

在 python 中 list 的合併十分簡單，可以直接使用 `+` 運算子直接合併：

```
>>> a = [1, 2, 3]
>>> b = [4, 5, 6]
>>> c = a + b
>>> c
[1, 2, 3, 4, 5, 6]
```

或者可以使用 `itertools` 模組。

當需要一次走訪 2 個很大的 list 時，就建議使用 itertools 的 chain 方法，
因為其使用 iterator 的方式將 2 個 list 內的元素走訪過一遍，不需額外建一個新的 list 存放所有元素，
而且 itertools.chain 接受任何 iterable 類型的變數作為參數，所以可以傳 tuple, list, string...。

```
>>> import itertools
>>> a = [1, 2, 3]
>>> b = [4, 5, 6]
>>> c = itertools.chain(a, b)
```


## 乘法的魔術

```
>>> a = [1, 2, 3]
>>> a * 3
[1, 2, 3, 1, 2, 3, 1, 2, 3]
```

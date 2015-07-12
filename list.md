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

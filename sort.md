## sort, sorted

sort 是 list 的方法，而 sorted 是內建的 function 。

sort 跟 sorted 最大不同點是 sorted 會另外 create 一塊記憶體放排序完的結果，而 sort 會直接改變 list 裡的次序。

此外 sorted 可以針對任何 iterable (sequence, list, set, dictionary ...) 進行排序

```
>>> a = [2, 1, 3, 5, 4]
>>> sorted(a)
[1, 2, 3, 4, 5]
>>> a
[2, 1, 3, 5, 4]
>>> a.sort()
[1, 2, 3, 4, 5]
>>> a
[1, 2, 3, 4, 5]
```

sort, sorted 都支援自定義的 key function 來排序， `key` 參數只接受 function 傳入，這個原理就是把每一個 iter 出來的 element 丟進 key function ，然後由 key function return 的作為依據進行排序。

例如，下列的 key function 其實是依序 return 'a', 'c', 'b', 'd', 'e' ，排序後就會變為 `(1, 'a'), (5, 'b'), (4, 'c'), (3, 'd'), (2, 'e')` ：

```

a = [(1, 'a'), (4, 'c'), (5, 'b'), (3, 'd'), (2, 'e')]

def key_sort(t):
    return t[1]

# 用英文字母排序，也就是第 2 個值
a.sort(key=key_sort)
# 更簡單的用法 使用 lambda
a.sort(key=lambda t: t[1])


print a
# result: [(1, 'a'), (5, 'b'), (4, 'c'), (3, 'd'), (2, 'e')]

```

在 python 2.5 之後，上述範例還可以用 operator 模組改寫，會效能更好一些：

```
from operator import itemgetter

a = [(1, 'a'), (4, 'c'), (5, 'b'), (3, 'd'), (2, 'e')]
a.sort(key=itemgetter(1))
```

如果是針對 class 排序還可以用 operator.attrgetter 進行排序。

而其實 itemgetter, attrgetter 其實回傳的就是 function

ref: https://docs.python.org/2/library/operator.html#operator.attrgetter

ref: https://wiki.python.org/moin/HowTo/Sorting

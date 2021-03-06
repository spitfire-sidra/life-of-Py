# Common mistakes

## enumerate

```
index = 1
for c in 'abcdefghi':
    if c == 'd':
        print 'index is {}'.format(index)
        break
    index +=1


# better
for index, c in enumerate('abcdefghi', 1):
    if c == 'd':
        print 'index is {}'.format(index)
        break
```

```
fruits = ['apple']
animals = ['cat', 'dog', 'elephant', 'bird']
for idx, value in enumerate(animals):
    if value not in fruits:
        del animals[idx]

print animals
# ['dog', 'bird'] <-- this is wrong


# right way
fruits = ['apple']
animals = ['cat', 'dog', 'elephant', 'bird']
for idx, value in reversed(list(enumerate(animals))):
    if value not in fruits:
        del animals[idx]
```

## is

is 比的是記憶體裡的位址，用記憶體位址判斷兩者是否相同。

```
An object’s identity never changes once it has been created; you may think of it as the object’s address in memory. The ‘is‘ operator compares the identity of two objects; the id() function returns an integer representing its identity (currently implemented as its address).
```

ref: https://docs.python.org/2/reference/datamodel.html#objects-values-and-types

例如：

```
>>> a = '1'
>>> b = '1'
>>> id(a)
4476400704
>>> id(b)
4476400704
>>> a is b
True


>>> a = 'Hello World'
>>> b = 'Hello World'
>>> id(a)
4477255088
>>> id(b)
4477255568
>>> a is b
False
```

## function without a return

事實上，function 裡如果沒有任何的 `return` ，在執行時，它還是會回傳一個 `None` 。

所以在實務上，最好是明確指定要回傳什麼，避免 `TypeError` 。

例如:

```
#
def clean_fruit(f=None):
    if f is not None:
        return f.strip()

s = clean_fruit()
s.replace('apple', 'banana')
# AttributeError: 'NoneType' object has no attribute 'replace'


# better way
def clean_fruit(f=None):
    if f is not None:
        return f.strip()

    # to return the default value
    return ''

s = clean_fruit()
s.replace('apple', 'banana')
```

```
In fact, even functions without a return statement do return a value, albeit a rather boring one. This value is called None (it’s a built-in name).
```

ref: https://docs.python.org/2/tutorial/controlflow.html#defining-functions

## mutable paramters

在使用 mutable objects 做為 function parameter 時要特別小心，因為 parameter 只會在解析時初始化一次，之後不管呼叫幾次，所使用的 parameter 都是解析時所建立的。

例如：

```

def dumplist(l=[]):
    l.append(1)
    print l

dumplist()
# [1]

dumplist()
# [1, 1] , 並不是預想中的 [1]
```

比較好的做法是：

```
def dumplist(l=None):
    if not l:
        l = []
    l.append(1)
    print l

dumplist()
# [1]

dumplist()
# [1]
```

ref: https://docs.python.org/2/tutorial/controlflow.html#default-argument-values

## bare except

下列程式請問會發生什麼事？

```
import sys

try:
    sys.exit()
except:
    print 'OOOPS!'

```

事實上，這段程式並不會讓程式結束，而是會進入到 `except` 區塊。

這是因為 `sys.exit()` 執行時會 `raise SystemExit()`

所以在處理 `try ... except` 時，最好盡量指明要處理何種 exception 。

例如：

```
import sys

try:
    sys.exit()
except StandardError:
    print 'OOOPS!'
```

## dict

```
def get_fruits(basket, fruit):
    try:
        return basket[fruit]
    except KeyError:
        return set()

# better
def get_fruit(basket, fruit):
    return basket.get(fruit, set())
```

do not pop/del element while iter dict

```
d = {'a': 1, 'b': 2, 'c':3}
for k, v in d.iteritems():
    d.pop('a')
    #RuntimeError: dictionary changed size during iteration
```


## set

```
def has_invalid_field(fields):
    for field in fields:
        if field not in ['foo', 'bar']:
            return true
    return False

# better
def has_invalid_fields(fields):
     return bool(set(fields) - set(['foo’, ‘bar']))
```


## Comprehension

```
set([i for i xrange(100000)])

# better ways
set(i for i xrange(100000))

{i for i xrange(1000000)}
```

## tuple to dictionary

```
tuples_in_list = [('key1', 'value1'), ('key2', 'value2'), ...]
mydict = {k: v for k, v in tuples_in_list}


# better way
mydict = dict(tuples_in_list)
```


## strip newline


```
>>> 'Mac EOL\r'.rstrip('\r\n')
'Mac EOL'
>>> 'Windows EOL\r\n'.rstrip('\r\n')
'Windows EOL'
>>> 'Unix EOL\n'.rstrip('\r\n')
'Unix EOL'

# better way
str = 'Windows EOL\r\n'
str.rstrip()
```

## `TypeError: 'in <string>' requires string as left operand, not NoneType`

```
>>> None in ('ABC')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'in <string>' requires string as left operand, not NoneType
```

right way

```
>>> None in ('ABC',)
False
```

## defaultdict

```
species = {}
for animal, family in {'cat': 'cat', 'tiger': 'cat', 'cat': 'cat', 'dog': 'dog'}.iteritems():
    if animal not in species:
        species[animal] = set()
    species[animal].add(animal)


# better
import collections
species = collections.default(set)
for animal in {'cat': 'cat', 'tiger': 'cat', 'cat': 'cat', 'dog': 'dog'}.iteritems():
    species.add(animal)
```

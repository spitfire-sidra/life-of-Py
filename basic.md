## 變數查找順序

```
The execution of a function introduces a new symbol table used for the local variables of the function. More precisely, all variable assignments in a function store the value in the local symbol table; whereas variable references first look in the local symbol table, then in the local symbol tables of enclosing functions, then in the global symbol table, and finally in the table of built-in names. Thus, global variables cannot be directly assigned a value within a function (unless named in a global statement), although they may be referenced.
```

因為 python 在 function 執行時還會建立一個 symbol table 來存 function 內的變數，所以 python 變數的查找順序：

1. 先在 function 內尋找變數
2. 找不到的話往 function 外尋找變數
3. 都找不到就找 built-in names

也就是因為 function 內有 symbol table ，所以不能直接在 function 內賦值給 global scope 的變數，除非在 function 內的變數有加上 `global` 的宣告。

example

```
var = 1

def func():
    var = 2
    print var

func() # print 2
print var # print 1
```


```
var = 1

def func():
    global var
    var = 2
    print var

func() # print 2
print var # print 2
```

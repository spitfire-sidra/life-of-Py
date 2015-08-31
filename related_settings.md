## pythonrc

```
$ export PYTHONSTARTUP="$HOME/.pythonrc”
```

可以把常用的func or 一些設定都寫在裡面

就不用每次都需要在python shell重新輸入


## PYTHONPATH

從 bash 修改 sys.path

```
PYTHONPATH="$HOME/your/module/path${PYTHONPATH+:}$PYTHONPATH"
export PYTHONPATH
```

從程式修改 sys.path

```
import sys

sys.path.append(‘/your/module/path’)
```


## pylint

產生一個 pylintrc

```
$ pylint --generate-rcfile > ~/.pylintrc
```

## flake8


```
$ touch ~/.config/flake8
```

example

```
[flake8]
ignore = E128,E203,E221,E226,E251,E261,E262,E265,E302,E501,E701
```

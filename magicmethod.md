`__all__`


為了避免 `from module import *` 把所有的 public class, variable, func 都 import 進來

可以在 `__init__.py` 或者 module 內部用一個 `__all__ = […]` 指定哪些可以被 import

例如:

```
# -*- coding: utf-8 -*-

__all__ = ["echo", "play", "stop"]

def echo():
    pass

def play():
    pass

def stop():
    pass

def rec():
    pass

```

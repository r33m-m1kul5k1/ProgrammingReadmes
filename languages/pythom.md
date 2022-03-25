# Python

# Packages
packages are modules that we want to import together.
### __init__.py
this file makes the current directory a python package

## args and kwargs
```python
# This function gets infinate number of parameters
def test(*args) -> None:
    pass

# This function get infinate number of key word parameters (dicts, etc...)
def test(**kwargs) -> None:
    pass
```
### Unpacking with args and kwargs
```python
array = *tuple_object
var1, var2, *array = tuple_object

var1, var2, *array, var3 = tuple_object
```
# Python inside a shell
to execute a command:
```Python
python -c 'command'
# example for a use (narnai0)
(python -c 'print 20*"A" + "\xef\xbe\xad\xde"'; cat;) | ./narnia0
```

# function symbols

# [logger](https://docs.python.org/3/howto/logging.html)
```
import logging
logging.info()
logging.warning()
logging.error()
logging.exception()
logging.critical()
```

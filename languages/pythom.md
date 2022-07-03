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

# function symbols & hints
## interable functions
- map -> change the iterable data with the given function
- filter -> construct a new iterator with all elements that returns true in the given function.

# [logger](https://docs.python.org/3/howto/logging.html)
```
import logging
logging.info()
logging.warning()
logging.error()
logging.exception()
logging.critical()

logging.basicConfig(format="%(levelname)s:%(asctime)s:%(thread)d - %(message)s", level=logging.DEBUG)
```

## Python classes
every class is an object. And its type(`__class__`) is type.\
you can even **add** attributes and methods to it, 
```py
A.new_atter = "sup"
A.new_method = exec
```
In order to add the dunder method `__getitem__` to a class the 
object must be a subscriptable `__class__`.
for that you need the to implement the metaclass (`__class__` / `type`) of the class

### Metaclasses
the description on how to create a class (the class of a class).
`type` is a metaclass. in order to change an objects metaclass
you need to do something like this
```py
class Meta(type):
    ...
        
class A(metaclass=Meta):
    ...

Meta.__getitem__ = exec
A["print('hello')"]
```
python uses metaclasses for creating the class definition as an object.



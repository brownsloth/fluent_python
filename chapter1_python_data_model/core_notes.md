1. Everything in python is an object
2. Python data model is how python treats various operations under the hood
```
for item in obj:
```
===>
```
iter(obj)
```
===>
```
obj.__iter__()
```
So a simple naive looking statement in python is actually a method called on some object.
Also, so if you implement certain methods in certain objects, you can make them behave certain ways!

3. Various constructs defined the data model:
3.1 Object creation and initialization

```
__new__() => creates new object
__init__() ==> initializes an object
```

3.2 String representation: `__repr__()` and `__str__()`

3.3 Arithmetic operators:
```
__add__()
__sub__()
__mul__()
__matmul__()
__truediv__()
```

3.4 Container/Collection behavior
```
__len__() == len(obj)
__getitem__() == obj[1]
__setitem__() == obj[1]=10
__contains__() == x in obj
```

Implementing these methods makes an object behave like a list!

3.5 iteration

```
__iter__() ==> creates an  iterator on the pbject
__next__() ==> defined on the iterator and fetches the next element from the object collection
```

`for x in obj:` ==> python first calls the `__iter__()` to create an iterator and then calls `__next__()` on start of every iteration
When the `__next__()` or `next(iter)` raises StopIteration exception, the for loop gracefully catches this and terminates the iteration.
So the for loop construct is doing much more than just calling these two dunder mehtods.

3.6 Truthiness
```
__bool__()
__len__()
```

For any construct, python can have a primary dunder that the object can define, and if not, then it looks for fallback dunder definintion. If neither are deinfed on the object we have a ultimate 
fallback or default behavior that python implements.

For truthiness: `__bool__` is primary, `__len__` is secondary (checks if `__len__` returns 0), Truthy (True) is the default.

3.7 Context managers: to manage resource allocation and deallocation

```
__enter__()
__exit__()
```

Ex:
```
with open('hello.txt', 'w') as f:
    f.write('Hello, World!')
```

open = a context manager class
open(..) = creates an object O of class open
with .. => calls `__enter__` on O
as f => assigns O ti variable f
at the end of the `...write..`, the `O.__exit__()` is called

3.8 Callable objects: Ex: model(x)

```
__call__()
```

3.9 Attribute access

```
__getattribute__ ==> primary dunder for getting attribute like o.abc
__getattr__ ==> fallback dunder 
```

```
__setattr__()
```


====

So basically, any object you (the developer) want to behave certain way, you implement the dunder methods. That way the usual syntax of python constructs is enough
to interact with your objects. Ex: Any object that implements collection method can enjoy the constructs of slicing, iteration etc.

The dunder methods are supposed to be called by the python interpreter and not by us devs directly.
The only special method that is frequently called by user code directly is __init__, to invoke the initializer of the superclass in your own __init__ implementation. Dunder methods follow the basic 
principles of inheritance just like regular methods.

 len(x) runs very fast when x is an instance of a built-in type. No method is called for the built-in objects of CPython: the length is simply read from a field in a C struct. len is not called as a 
method because it gets special treatment as part of the Python data model, just like abs. But thanks to the special method __len__, you can also make len work with your own custom objects.

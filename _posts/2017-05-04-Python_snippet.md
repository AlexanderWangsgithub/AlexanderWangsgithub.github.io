



### Field

```python
## eg1:
def countIter(n):
    while n>0:
        yield n
        n -= 1
    print("next end")

for i in countIter(5):
    print(i)

# "yield" key mean return a next(), when a iterator was used, next() would be used.
# One benefit of "yield" is void to store data with a list, save memory.
# 05-04: when you execute countIter() return a iterator. So, 'for i in countIter(5)' means countIter.next...

## eg2:
def read_file(fpath):
    BLOCK_SIZE = 1024
    with open(fpath, 'rb') as f:
        while True:
            block = f.read(BLOCK_SIZE)
            if block:
                yield block
            else:
                return
```

使用field的函数，返回迭代器，该函数并不先执行获得结果而是在使用的使用执行。

见下面例子，我们并没有得到一个[5,4,3,2,1]，而是一个迭代器，迭代到3的时候break，并没有执行一个函数获得[5,4,3,2,1]。

```python
## eg3:
for i in countIter(5):
    if( i<3 ):
        break;
    print(i)
# result: 5,4,3
```



### xrange()

和field一样，这也是个懒加载函数，range()会直接返回一个链表，而xrange()是使用时才会生成。



### Type

```python
class A:
    pass

class B(A):
    pass

isinstance(A(), A)  # returns True
type(A()) == A      # returns True
isinstance(B(), A)    # returns True
type(B()) == A        # returns False
```

type方法不把子类当作是父类，isinstance中子类实例是父类。



### for

```
array = [1, 2, 3, 4, 5]
for i, e in enumerate(array):
# index 'i' and its corresponding element 'e'.
```



### 序列乘法

写测试时候常用。

```
nums = [0] * 100    # [0,0,0,....]
strs = 100 * "a"    # "aaaaaa..."
```



### object

```python
class object:
    """ The most base type """
    def __delattr__(self, *args, **kwargs): # real signature unknown
        """ Implement delattr(self, name). """
        pass

    def __dir__(self): # real signature unknown; restored from __doc__
        """
        __dir__() -> list
        default dir() implementation
        """
        return []
    def __getattribute__(self, *args, **kwargs): # real signature unknown
        """ Return getattr(self, name). """
        pass
    @staticmethod # known case of __new__
    def __new__(cls, *more): # known special case of object.__new__
        """ Create and return a new object.  See help(type) for accurate signature. """
        pass
    def __setattr__(self, *args, **kwargs): # real signature unknown
        """ Implement setattr(self, name, value). """
        pass

    __class__ = None # (!) forward: type, real value is ''
    __dict__ = {}
    __doc__ = ''# 文档字符串，object和function都有
    __module__ = '' # code单元归属
```

object中attribute的存储在`__dict__`里面，"For instance, `a.x` has a lookup chain starting with `a.__dict__['x']`, then `type(a).__dict__['x']`, and continuing through the base classes of `type(a)` excluding metaclasses. "。

和java object差别是，java中class field在编译期间就声明了，py中可以`__setattr__`，这个特性在mock数据时候很有用。

[About discriptor](https://docs.python.org/2/howto/descriptor.html)

关于[Python DataModel](https://docs.python.org/3/reference/datamodel.html)，写的很赞。



### 生成式

列表生成式是循环的语法糖，来自Haskell，相当于lambda-map+filter。

```python
results = [x for x in range(20) if x % 2 == 0]
# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

集合、字典生成式

```
>>> # Set Comprehensions
>>> some_list = [1, 2, 3, 4, 5, 2, 5, 1, 4, 8]
>>> even_set = { x for x in some_list if x % 2 == 0 }
>>> even_set
set([8, 2, 4])

>>> # Dict Comprehensions
>>> d = { x: x % 2 == 0 for x in range(1, 11) }
>>> d
{1: False, 2: True, 3: False, 4: True, 5: False, 6: True, 7: False, 8: True, 9: False, 10: True}
```
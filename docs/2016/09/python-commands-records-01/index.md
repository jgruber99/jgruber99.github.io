# Python使用记录-学习笔记-01


# 学习自[廖雪峰的官方网站][liaoxuefeng]--Python教程

## Python的高级特性

### 切片
> 创建一个0-99的数列

```
>>> L = list(range(100))
>>> L
[0, 1, 2, 3, ..., 99]
```
	
> 前10个数：

```
>>> L[:10]
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```
	
> 后10个数：

```
>>> L[-10:]
[90, 91, 92, 93, 94, 95, 96, 97, 98, 99]
```

> 前11-20个数：

```
>>> L[10:20]
[10, 11, 12, 13, 14, 15, 16, 17, 18, 19]
```

<!-- more -->

> 前10个数，每两个取一个：

```
>>> L[:10:2]
[0, 2, 4, 6, 8]
```

> 所有数，每5个取一个：

```
>>> L[::5]
[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]
```

> 原样复制list

```
>>> L[:]
[0, 1, 2, 3, ..., 99]
```

> 倒转list

```
>>> L[::-1]
[99, 98, 97, 96, ..., 0]
```

> tuple切片仍是tuple：

```
>>> (0, 1, 2, 3, 4, 5)[:3]
(0, 1, 2)
```

> 字符串也可以当作list来切片

```
>>> 'ABCDEFG'[:3]
'ABC'
>>> 'ABCDEFG'[::2]
'ACEG'
```

### 迭代

> 通过``for ... in``循环来遍历list或tuple，这种便利称为迭代(iteration)。

#### 迭代dict

> 默认dict迭代的是key

```
>>> d = {'a': 1, 'b': 2, 'c': 3}
>>> for key in d:
...    print(key)
...
a
c
b
```

> 迭代dict的value

```
>>> for value in d.values():
...    print(value)
...
1
2
3
```

> 同时迭代key和value

```
>>> for k, v in d.items():
...		print(k, v)
...
b 2
c 3
a 1
```

#### 判断一个对象是否可迭代

> 通过collections模块的Iterable类型判断：

```
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str是可迭代对象
True
>>> isinstance([1, 2, 3], Iterable) # list是可迭代对象
True
>>> isinstance(123, Iterable) # 整数不是可迭代对象
False
```

#### list实现下标循环

> 使用Python内置的``enumerate``函数实现

```
>>> for i, v in enumerate(['A', 'B', 'C']):
...		print(i, v)
...
0 A
1 B
2 C
```

### 列表生成式

> 简单方便的生成list

```
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```
```
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```
```
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```
```
>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> [k + '=' + v for k, v in d.items()]
['y=B', 'x=A', 'z=C']
```
```
>>> L = ['Hello', 'World', 'IBM', 'Apple']
>>> [s.lower() for s in L]
['hello', 'world', 'ibm', 'apple']
```

### 生成器

> 把列表生成式的``[]``改成``()``就可以创建一个生成器g
> 
> 通过``next(g)``打印出来

#### 创建一个斐波拉契数列的生成器

```python
def fib(max):
	n, a, b = 0, 0, 1
	while n < max:
		yield b
		a, b = b, a + b
		n += 1
	return 'done'
```

### 迭代器

```
>>> from collections import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
>>> isinstance([], Iterator)
False
>>> isinstance({}, Iterator)
False
>>> isinstance('abc', Iterator)
False
```

- 凡是可作用于``for``循环的对象都是``Iterable``类型；
- 凡是可作用于``next()``函数的对象都是``Iterator``类型，它们表示一个惰性计算的序列；
- 集合数据类型如``list``、``dict``、``str``等是``Iterable``但不是``Iterator``，不过可以通过``iter()``函数获得一个``Iterator``对象。


```
from collections import Iterator
>>> isinstance(iter([]), Iterator)
True
>>> isinstance(iter('abc'), Iterator)
True
```

### 函数式编程

#### 高阶函数

##### map/reduce

> ``map``函数：把第一个参数(函数)，作用在第二个参数(Iterable)的每个元素上，并把结果作为新的``Iterator``返回。
> 
> 例子:

```
>>> list(map(str, [1, 2, 3, 4, 5, 6, , 7, 8, 9]))
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

> ``reduce``函数：第一个参数是函数，第二个参数是序列，函数依次作用于序列的每一个元素，并把结果作用于下一个元素
> 
> 例子：

```
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

##### filter

> 过滤序列。第一个参数是函数，第二个参数是序列，函数依次作用于序列的每一个元素，然后根据返回值``True``还是``False``决定保留还是丢弃该元素。
> 
> 又是惰性的。

```python
def is_odd(n):
	return n % 2 == 1
list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
```
```
# 结果: [1, 5, 9, 15]
```

##### sorted

> ``sorted()``也是一个高阶函数。用``sorted()``排序的关键在于实现一个映射函数。

```
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```

#### 返回函数

> 一个函数返回另一个函数，不立即执行，调用时执行。

#### 匿名函数

> ``lambda``表达式。

```
f = lambda x : x * x
```

#### 装饰器

> 动态增加函数的功能。

```python
import functools
	
def log(text):
def decorator(func):
	@functools.wraps(func)
	def wrapper(*args, **kw):
		print('%s %s():' % (text, func.__name__))
		return func(*args, **kw)
	return wrapper
return decorator
```

#### 偏函数

> 当函数的参数个数太多，需要简化时，使用``functools.partial``可以创建一个新的函数，这个新函数可以固定住原函数的部分参数，从而在调用时更简单。

```
>>> import functools
>>> int2 = functools.partial(int, base=2)
>>> int2('1000000')
64
>>> int2('1010101')
85
```

### 面向对象编程

#### 访问限制

> 定义私有变量，需要在变量前加上两个下划线``__``，通过get方法获取，或者通过set方法赋值。
> 
> 不能直接访问``__name``是因为Python解释器对外把``__name``变量改成了``_Student__name``，所以，仍然可以通过``_Student__name``来访问``__name``变量
> 
> 强烈不建议这么干，不同版本的Python解释器可能会改成不同的变量名。
> 
> 需要注意的是，在Python中，变量名类似``__xxx__``的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量，所以，不能用``__name__``、``__score__``这样的变量名。

#### 获取对象信息

##### type()

```
>>> import types
>>> def fn():
...     pass
...
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True
>>> type((x for x in range(10)))==types.GeneratorType
True
```

##### isinstance()

- 判断class的继承关系
- 也能替代``type()``判断基本类型
- 还可以判断一个变量是否是某些类型中的一种

```
>>> isinstance([1, 2, 3], (list, tuple))
True
>>> isinstance((1, 2, 3), (list, tuple))
True
```

##### dir()

> 如果要获取一个对象的所有属性和方法，可以使用``dir()``函数，它返回一个包含字符串的list.
> 
> 仅仅把属性和方法列出来是不够的，配合``getattr()``、``setattr()``以及``hasattr()``，我们可以直接操作一个对象的状态。

### 面向对象高级编程

#### 使用``__slots__``

> 正常可以动态绑定实例的属性和方法，如果在实例中定义一个特殊的``__slots__``变量，则可以限制该class实例能添加的属性。
> 
> 使用``_slots__``要注意，``__slots__``定义的属性仅对当前类实例起作用，对继承的子类不起作用。

#### 使用``@property``

> Python内置的``@property``装饰器就是负责把一个方法变成属性调用。
> 

```python
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

> 如果只定义了get方法，没有定义set方法，就是只读属性

#### 定制类

> 需要特殊变量
> 
- ``__slots__``
- ``__len__``
- ``__str__``
- ``__repr__``
- ``__iter__``
- ``__next__``
- ``__getitem__``
- ``__getattr__``
- ``__call__``

#### 使用元类

``__metaclass__``

> 理解难点，找到了一个详细讲解的文章
> 
> [深刻理解Python中的元类(metaclass)][jobbole_python_metaclass]
> 
> 此文章翻译自``stackoverflow``的一个神回复
> 
> [what-is-a-metaclass-in-python][stackoverflow-python-metaclass]

### 错误、调试、和测试

#### 错误处理

> 捕获异常并处理

```
try...except...finally...
```

> 主动抛出异常

```
raise
```

#### 调试

##### 断言

> 断言返回False的，则抛出``AssertionError``

```
assert n != 0, 'n is zero!'
```

> 启动Python解释器时可以用``-0``参数来关闭``assert``

```
python3 -0 err.py
```

#### logging

#### pdb

> 让程序以单步方式运行
> 
- 以参数``-m pdb``启动
- 输入命令``n``可以单步执行代码
- 输入命令``p 变量名``来查看变量
- 输入命令``q``结束调试，退出程序

> 也可以设置断点
> 
- 需要先``import pdb``
- 在需要断点的地方放置``pdb.set_trace()``

### IO编程

#### StringIO和BytesIO

#### 操作文件和目录

##### ``OS``模块

```
>>> import os
>>> os.name # 操作系统类型
>>> os.uname() # 详细的系统信息
>>> os.environ() # 系统环境变量
>>> os.path.abspath('.') # 当前目录绝对路径
>>> os.path.join('/Users/Bruce', 'testdir') # 拼接路径
>>> os.mkdir('/Users/Bruce/testdir') # 创建目录
>>> os.rmdir('/Users/Bruce/testdir') # 删除目录
>>> os.path.split('/Users/Bruce/testdir/file.txt')
('/Users/Bruce/testdir', 'file.txt')
>>> os.rename('test.txt', 'test.py') # 对文件重命名
>>> os.remove('text'.py') # 删除文件
```

### 常用内建模块

> 
+ datetime
+ collections
	- namedtuple
	- depue
	- defaultdict
	- OrderedDict
	- Counter
+ base64
+ hashlib
+ itertools
	- count()
	- cycle()
	- repeat()
	- takewhile()
	- chain()
	- groupby()
+ XML
	- DOM
	- SAX
+ HTMLParser
+ urllib

### 常用第三方模块

#### PIL

> PIL:Python Imaging Library，强大的图片处理模块
> 
> Pillow，在PIL的基础上创建的兼容的版本

### 异步IO

#### 协程

> 没有理解，重点看一下。

#### asyncio

#### async/await

#### aiohttp







[liaoxuefeng]: http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000

[jobbole_python_metaclass]: http://blog.jobbole.com/21351/

[stackoverflow-python-metaclass]: http://stackoverflow.com/questions/100003/what-is-a-metaclass-in-python

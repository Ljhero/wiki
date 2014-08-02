---
title: "《Python 核心编程》阅读笔记"
date: 2014-08-02 22:37:11
---

## 特点

与其他语言相比，python具有以下特点：

* 高级。内建于语言本身的高级数据结构，如list，dict。
* 面向对象。具有与生俱来的面向对象特性，同时实现了一些函数式语言特性，如lambda表达式。
* 可升级。提供了基本的开发模块，具有可插入性和模块化架构。
* 可扩展。统一的扩展接口，不仅支持python语言扩展，还支持C、C++等其他语言编写的扩展。
* 可移植性。解释型语言，可运行于不同的架构及不同的操作系统。
* 易学，易读。

## py脚本自动执行python解释器

在类Unix平台，在脚本的第一行使用shell魔术字符串（“sh-bang”）

```
#!/usr/local/bin/python
```

如果脚本文件具有执行权限，可以直接运行，不需要明确制定python解释器

```
python script.py ---> ./script.py
```

在许多Unix系统中有一个命令叫env, 位于/bin 或 /usr/bin中。它会自动在系统中找到python解释器。所以可以使用一个更好的方式指定python解释器。

```
#!/usr/bin/env python
或
#!/bin/env python
```

在Windows系统中，通过文件关联实现自动运行。所以只要是.py扩展名的脚本，直接运行会自动调用python解释器，并不需要添加魔术行。

注意在Windows下编写的脚本，行结尾是CRLF形式。如果添加了魔术行在Linux系统上直接执行，会报*Not such file or directory*错误。Linux行结尾为LF，所以魔术行被识别为*#!/usr/bin/env python\r*，也就在系统中寻找的是*python\r*而不是*python*解释器。

## ++与--

Python 不支持C语言汇总的自增与自减运算。++和--被解释为两个正号和负号的单目运算符。在Python中负号(-)运算符会改变数值符号，而正号不改变。
--n会解释为-(-n)从而得到n，同样++n的结果也是n。n的值未改变。
使用 n += 1 或 n -= 1代替。

## 无限制的名称空间

可以给函数或类实例添加任意属性。

```
def foo():
     pass
foo.x = 1

class MyClass(object):
     pass
c = MyClass()
c.y = 2
```

使用 del 删除属性：

```
del foo.x
```

## import 与 from - import

import 是导入模块。输入包名不会自动导入包中的模块名称。除非包中的__init__.py 文件中使用了from..import语句导入了包中的模块。

from - import 从包中导入模块或是模块中导入指定的名称。

不提倡使用 from xx import *。推荐使用多行导入指定名称，并使用圆括号来创建多行导入语句

```
from xx import （y1, y2,
                    z3, z4)
```

如果你不想让某个模块属性被 "from module import *" 导入 , 那么你可以给你不想导入的属性名称加上一个下划线( _ )。 不过如果你导入了整个模块或是你显式地导入某个属性(例如 import foo._bar ), 这个隐藏数据的方法就不起作用了。

## 关于 `__future__`

为了让Python程序员为新事物做好准备，Python 实现了 __future__指令。

使用 from-import语句导入新特性，

```
from __future__ import new_feature
```

## 内建函数

repr(obj) 或 `obj` 返回一个对象的字符串表示, 绝大多数情况下可以通过eval()重新获得该对象。
str(obj)                  返回对象适合可读性好的字符串表示，输出对人比较友好，适合print语句。

## 序列切片

可以用None作为索引值。
s[:None] == s[:]

## 字符串连接

可以使用括号连接多行字符串

```
s = ('hello'
     'world')
print s
'helloworld'
```

如果调用函数返回的字符串连接还是需要 加号，但因括号的存在不需要加反斜杠。

```
s = (s.replace('s', 'x') +
     'world')
```

## Unicode

使用应用程序完全支持Unicode，遵守以下规则。

* 程序中出现字符串时一定要加个前缀u。
* 不要用str()函数，用unicode()代替。
* 不到必须时不要再程序里面编解码Unicode字符，只在用写入文件或数据库或者网络时，才调用encode()函数；相应地需要从外部读入数据时调用decode()函数解码为unicode字符。

pickle模块值只支持ASCII字符串。现在二进制格式已为pickle的默认格式。所以在操作pickle数据时直接存储unicode二进制形式，再读入时已二进制形式直接转换为Unicode字符。向数据库写入时，使用BLOB字段存储，而不用TEXT或VARCHAR。

## 列表拷贝

列表切片，list() 和使用copy模块的copy函数，默认都是浅拷贝，对于不可变元素生成一个新的对象，而对于可变元素只是复制引用。

如果需要得到一个完全拷贝或者说深拷贝--创建一个新的容器对象，则需要使用copy模块的deepcopy()函数。

## 字典

setdefault()函数，检查字典中是否含有某键。如果字典中这个键存在，返回它的值。如果不存在，则给这个键赋默认值并返回此值。

```
>>> d = {'k': 10}
>>> d.setdefault('k', 20)
10
>>> d.setdefault('m', 10)
10
>>> d
{'k':20, 'm':10}
```

### else 语句

else 可以在while和for循环中使用。else语句只在循环完成后执行，break语句会跳过else块。

在try...except 中使用else，只有没有产生异常时，else块才会被执行。

## 迭代器

使用( ... ) 生成迭代器。

迭代器有一个next()方法的对象，不是通过索引来计数。通过调用迭代器的next()方法，获取下一项。

对一个对象调用iter()可以得到它的迭代器。
iter(obj)
iter(func, sentinel) 它会重复调用func迭代产生数据项，直到下个值等于sentinel。


## 函数多元返回

对于函数返回多个对象，python是把他们聚集起来并以一个元组返回，实际还是返回一个对象，只是元组语法上不需要一定带上圆括号。

如果没有显示返回值，默认返回None。

```
def foo():
     return 'abc', 12, [12,11]

aTuple = bar()
x, y, z = bar()
(a, b, c) = bar()
```

## 调用带有可变长参数对象函数

```
def newfoo(arg1, *arg, **arg_dict):
     pass
```
一般方式
```
newfoo(1, 2, 3, arg2=1, arg3=1)
```
也可以先放在元组和字典中
```
newfoo(1, *(2, 3), **{'arg2': 1, 'arg3':1})
```
还可以在调用前先创建元组和字典
```
arg_tupple = (2, 3)
arg_dict = {'arg1': 1, 'arg3': 1}
newfoo(1, *arg_tupple, **arg_dict)
```

## 偏函数应用

一个带n个参数的函数，固化某些参数，并返回另一个带剩下参数的函数对象。
使用functools模块中的partial()函数

```
from operator import add
from functools import partial

add1 = partial(add, 1)  # add1(x) == add(1, x)

baseTwo = partial(int, base=2) # baseTwo(x) == int(x, 2)
```
## `__init__()` 与 `__new__()`

__init__()方法，是类实例被创建后默认调用的第一个方法，它需要self参数也表明实在实例化之后才能调用。另外子类中覆盖了__init__()方法并不会自动调用父类的__init__()方法。需要显示调用

```
super(C, self).__init__();
```

__new__()方法，一个类方法调用时第一个参数为类，它也更像类的构造器。解释器在创建对象时调用，并返回一个合法的实例。

## `__getattr__()` 和 `__getattribute__()`

`__getattr__()` 但属性不能再实例的`__dict__`或它的类，或者祖先类中找到时，才被调用。
`__getattribute__()` 当属性被访问时就被调用。

## 元类 `__metaclass__`

元类用于创建类，也就是类在定义时执行。
类的`__metaclass__`属性指定元类，在执行类定义的时候，将检查此类的正确。
元类的构造器接受四个参数：类，类的名称，从基类继承数据的元组，和（类的）属性字典。

## 类的`__call__`方法

实现`__call__`方法，使类的实例成为可调用的。

```
class C(object):
     def __call__(self, *args):
          print "I'm callable!\n", args
>>> c = C()
>>> c(3)
I'm callable!
3
```

## 正则表达式

使用compile()编译正则表达式。在模式匹配前，正则表达式模式必须先被编译成regex对象，由于正则表达式在执行过程中被多次用于比较，所以为了提高性能建议箱对模式进行预编译。

模式字符串使用前缀r （raw）。在python中字符串有前缀r，表示其为原始字符串，其中的反斜杠不会当作特殊字符。例如r'\n'，代表两个字符而不会当作一个换行符。
如果不需要匹配ASCII中的特殊字符，建议模式字符串使用原始字符串形式。
如果不使用原始字符串，ASCII中有与正则表达式相对应的特殊字符，则要使用反斜线进行转义。
例如 '\b' 在ASCII中表示退格，在正则中表示单词开始。
‘\bblow' 则匹配的是退格键之后是blow的字符串
’\\bblow' 和 r'\bblow' 都是匹配以blow开始的单词。

## 多线程编程

使用threading 模块

### 守护线程

当主线程退出时，所有子线程不论它们是否还在工作，都会被强行退出。为了避免这种情况引入守护线程的概念。
如果一个线程为守护线程，则说明这个线程是不重要的，在进程退出的时候，不用等候这个线程退出。通过调用setDaemon()函数设定相册的daemon标志，新的子线程会继承其父线程的daemon标志。
整个Python会在所有的非守护线程退出后才会结束，即进程中没有非守护线程运行的时候才结束。

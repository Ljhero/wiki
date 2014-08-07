---
title: "Python 中的函数与类的方法"
date: 2014-08-05 21:00:35
---

> 注：本文转译自 Stackoverflow 上 [Adding a Method to an Existing Object](http://stackoverflow.com/questions/972/adding-a-method-to-an-existing-objec) 的最佳回答。

在 python 中，def 定义的函数与类中的方法有很大的不同，两者是不同的类型。

```python
>>> def foo():
...     print "foo"
...
>>> class A:
...     def bar( self ):
...         print "bar"
...
>>> a = A()
>>> foo
<function foo at 0x00A98D70>
>>> a.bar
<bound method A.bar of <__main__.A instance at 0x00A9BC88>>
>>>
```

类中的方法是绑定方法，会具体绑定到某一类的实例。当方法被调用时，实例对象会作为第一个参数(self)，被传入到方法中。

一个类中的可调用属性一直是未绑定，当类被实例化为一个对象时才绑定到某一具体实例上。所以我们可以在任何时候对类中已定义的方法进行修改。

```python
>>> def fooFighters( self ):
...     print "fooFighters"
...
>>> A.fooFighters = fooFighters
>>> a2 = A()
>>> a2.fooFighters
<bound method A.fooFighters of <__main__.A instance at 0x00A9BEB8>>
>>> a2.fooFighters()
fooFighters
```

在修改之前已经实例化的对象也会改变（只要它们没有覆盖自身属性）。新添加的方法会自动与实例对象进行绑定。

```
>>> a.fooFighters()
fooFighters
```

这种直接添加或修改类中的方法只能在类上进行修改，如果只想给一个实例化的对象增加方法就会产生问题。

```
>>> def barFighters( self ):
...     print "barFighters"
...
>>> a.barFighters = barFighters
>>> a.barFighters()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: barFighters() takes exactly 1 argument (0 given)
```

当把方法直接添加到一个实例对象时，函数并没有自动与与实例对象进行绑定。其仍然是一个函数类型，而不是一个绑定方法 （bound method）。

```
>>> a.barFighters
<function barFighters at 0x00A98EF0>
```

我们可以使用 [`types` 模块中的 `MethodType`][1] 函数显示绑定函数到某一个实例对象上。

```
>>> import types
>>> a.barFighters = types.MethodType( barFighters, a )
>>> a.barFighters
<bound method ?.barFighters of <__main__.A instance at 0x00A9BC88>>
>>> a.barFighters()
barFighters
```

这样修改只改变了实例 a 的方法，不会影响到其他实例。

[1]: http://docs.python.org/library/types.html?highlight=methodtype#module-types

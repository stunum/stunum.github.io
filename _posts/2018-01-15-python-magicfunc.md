---
layout: post
title: python中__init__()、 __call__()、 __new__()、 __del__()方法的作用
date: 2018-01-15 19:44:38
tags:  水滴石穿
---

根据python的对象从创建到销毁的顺序讲
`__new__()` -> `__init__()` -> `__call__()` -> `__del__()`
```
class Person(object):
    def __new__(cls,[,*args [,**kwargs]]):
        return super().__new__(cls,[,*args [,**kwargs]])

    def __init__(self,[,*args [,**kwargs]]):
        ...

    def __call__(self,[,*args [,**kwargs]]):
        ...

    def __del__(self,[,*args [,**kwargs]]):
        ...

```
- **`__new__()`** 方法:
负责对象的创建，是一个静态方法，第一个参数是`cls`。
  1. 实例化对象时，一定被调用。
  2. 如果 `__new__()` 方法不返回值（或者说返回 None）那么 `__init__()`  将不会得到调用

- **`__init__()`** 方法:
初始化对象内属性的方法，只有在对象创建完毕需要初始化属性时调用。`__init__()`方法 只能返回 None 值

- **`__call__()`** 方法:
首先只有对象才是可以`call`的，当生成的对象被像函数一样调用时就会触发。

- **`__del__()`** 方法:
在对象的生命周期结束时即该对象的所有引用都被删除之后, `__del__()` 会被调用,可以将 `__del__()` 理解为"析构函数"

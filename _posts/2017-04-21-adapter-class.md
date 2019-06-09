---
layout: post
title: python设计模式之适配器模式
date: 2017-04-21 20:47:19
tags:  水滴石穿
---
### 适配器模式

##### 意义：

将一个类的接口转换成客户希望的另外一个接口。Adapter 模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
##### 适用性：
- 你想使用一个已经存在的类，而它的接口不符合你的需求。

- 你想创建一个可以复用的类，该类可以与其他不相关的类或不可预见的类（即那些接口可能不一定兼容的类）协同工作。

- （仅适用于对象Adapter ）你想使用一些已经存在的子类，但是不可能对每一个都进行子类化以匹配它们的接口。对象适配器可以适配它的父类接口。

##### 举例：
```
class A(object):
    def a(self):
        print("我是A类的a方法")

class B(object):
    def b(self):
        print("我是B类的b方法")

class C(object):
    def c(self):
        print("我是C类的c方法")

class Adapter(object):
    def __init__(self, classname, method):
        self.classname = classname
        self.__dict__update = method
    def __getattr__(self, attr):
        return getattr(self.classname, attr)

def test():
    objects = []
    AA = A()
    objects.append(Adapter(AA, dict(test=AA.a)))
    BB = B()
    objects.append(Adapter(BB, dict(test=BB.b)))
    CC = C()
    objects.append(Adapter(CC, dict(test=CC.c)))
    for obj in objects:
        obj.test()

test()
```
输出结果：

```
我是A类的a方法
我是B类的b方法
我是C类的c方法
```
核心思想是创建一个适配器类，通过__dict__将需要转化的类的方法注册到适配器，复写__getattr__使其在适配器函数查无方法的时候，执行getattr魔法方法。
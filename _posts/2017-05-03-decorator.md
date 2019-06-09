---
layout: post
title: python设计模式之装饰器模式
date: 2017-05-03 09:01:47
tags:  水滴石穿
---
### 装饰器模式

##### 意义：
动态地给一个对象添加一些额外的职责。就增加功能来说，Decorator 模式相比生成子类更为灵活。

##### 适用性
- 在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。
- 处理那些可以撤消的职责。
- 当不能采用生成子类的方法进行扩充时。一种情况是，可能有大量独立的扩展，为支持每一种组合将产生大量的子类，使得子类数目呈爆炸性增长。另一种情况可能是因为类定义被隐藏，或类定义不能用于生成子类。

##### 举例：
```
class Test(object):
    def add(self):
        print("添加数据")

    def remove(self):
        print("删除数据")

class Decorator(object):
    def __init__(self, name):
        self._run = name

    def save(self):
        print("保存数据")

    def __getattr__(self, name):
        return getattr(self._run, name)

if __name__=='__main__':
    testA=Test()
    testB=Decorator(testA)
    testB.remove()
    testB.add()
    testB.save()
```
输出结果：
```
删除数据
添加数据
保存数据
```
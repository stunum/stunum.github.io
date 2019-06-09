---
layout: post
title: python设计模式之原型模式
date: 2017-04-12 23:55:24
tags:  水滴石穿
---
### 原型模式

##### 意义：
用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。
##### 适用性：
当要实例化的类是在运行时刻指定时，例如，通过动态装载；或者为了避免创建一个与产品类层次平行的工厂类层次时；或者当一个类的实例只能有几个不同状态组合中的一种时。建立相应数目的原型并克隆它们可能比每次用合适的状态手工实例化该类更方便一些。
##### 举例：
```
import copy

class Information(object):
    """个人信息"""
    def __init__(self):
        self.name = None
        self.ager = None
        self.height = None

    def run(self):
        """
        自我介绍方法
        """
        print(f"我叫：{self.name} 年龄：{self.ager} 身高：{self.height}")

class Prototype(object):
    def __init__(self, obj):
        self.copy_object = obj()

    def clone(self, **attr):
        obj = copy.deepcopy(self.copy_object)
        obj.__dict__.update(attr)
        return obj

if __name__ == '__main__':
    test = Prototype(Information)
    a = test.clone(name='张山', ager="30", height='170cm')
    a.run()
    b = test.clone(name='李飞', ager="20", height='190cm')
    b.run()
```

输出结果：
```
我叫：张山 年龄：30 身高：170cm
我叫：李飞 年龄：20 身高：190cm
```
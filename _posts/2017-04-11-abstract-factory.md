---
layout: post
title: python设计模式之抽象工厂
date: 2017-04-11 16:58:16
tags:  水滴石穿
---
### 抽象工厂

##### 意义：
提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。
##### 适用性：
* 一个系统要独立于它的产品的创建、组合和表示时。
* 一个系统要由多个产品系列中的一个来配置时。
* 当你要强调一系列相关的产品对象的设计以便进行联合使用时。
* 当你提供一个产品类库，而只想显示它们的接口而不是实现时。

##### 举例：
```
class A(object):
    def __init__(self):
        self.word = "运行A"
    def run(self):
        print(self.word)

class B(object):
    def __init__(self):
        self.word = "运行B"
    def run(self):
        print(self.word)

class Interface(object):
    def __init__(self, classname=None):
        self.name = classname

    def run(self):
        self.name().run()

if __name__ == "__main__":
    test = Interface()
    test.name = "A"
    test.run()
    test.name = "B"
    test.run()

```

输出结果：

```
运行A
运行B
```

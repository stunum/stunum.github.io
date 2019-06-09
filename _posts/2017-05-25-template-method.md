---
layout: post
title: python设计模式之模板方法模式
date: 2017-05-25 18:06:58
tags:  水滴石穿
---
### 模板方法模式

##### 意义：
定义一个算法或者流程，部分环节设计为外部可变，用类似于模板的思想来实例化一个实体，可以往模板中填充不同的内容；在模板思想下，实体的整体框架是确定的，他是一个模板，但是模板下内容可变，从而实现了动态的更新流程或算法。
##### 适用性：
- 一次性实现一个算法的不变的部分，并将可变的行为留给子类来实现。
- 各子类中公共的行为应被提取出来并集中到一个公共父类中以避免代码重复。这是Opdyke 和Johnson所描述过的“重分解以一般化”的一个很好的例子[ OJ93 ]。首先识别现有代码中的不同之处，并且将不同之处分离为新的操作。最后，用一个调用这些新的操作的模板方法来替换这些不同的代码。
##### 举例：
```
#实现一个客户点单后的处理流程流程
class User(object):

    def __init__(self, name, shop, times, number):
        self.name = name
        self.shop = shop
        self.times = times
        self.number = number


class Handle(object):

    def __init__(self, user=None):
        self.user = user

    def Invoicen(self):
        #打印小票
        string = f"打印小票  客户：{self.user.name}  商品：{self.user.shop}  数量：{self.user.number}  时间：{self.user.times}"
        print(string)

    def Make(self):
        #开始制作
        print(f"制作完成：{self.user.shop} 数量：{self.user.number}")

    def run(self):
        self.Invoicen()
        self.Make()


if __name__ == '__main__':
    test = Handle()
    xiaoming = User("小明", "汉堡", "17:50", "5")
    test.user = xiaoming
    test.run()
```

输出结果：
```
打印小票  客户：小明  商品：汉堡  数量：5  时间：17:50
制作完成：汉堡 数量：5
```
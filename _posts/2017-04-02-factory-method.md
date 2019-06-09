---
layout: post
title: python设计模式之工厂模式
date: 2017-04-02 18:52:14
tags:  水滴石穿
---

### 工厂模式

##### 意义：

定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method 使一个类的实例化延迟到其子类。

##### 适用性：

* 当一个类不知道它所必须创建的对象的类的时候。
* 当一个类希望由它的子类来指定它所创建的对象的时候。
* 当类将创建对象的职责委托给多个帮助子类中的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时候。

##### 举例：

```
#python3
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

def Interface(classname):
    run = dict(A=A, B=B)
    return run[classname]()

if __name__ == "__main__":
    testA,testB = Interface("A"),Interface("B")
    testA.run()
    testB.run()
```

输出结果：

```
运行A
运行B
```

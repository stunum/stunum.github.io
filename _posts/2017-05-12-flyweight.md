---
layout: post
title: python设计模式之享元模式
date: 2017-05-12 17:36:14
tags:  水滴石穿
---
### 享元模式

##### 意义：
运用共享技术有效地支持大量细粒度的对象。
##### 适用性：
- 一个应用程序使用了大量的对象。
- 完全由于使用大量的对象，造成很大的存储开销。
- 对象的大多数状态都可变为外部状态。
- 如果删除对象的外部状态，那么可以用相对较少的共享对象取代很多组对象。 
- 应用程序不依赖于对象标识。由于Flyweight 对象可以被共享，对于概念上明显有别的对象，标识测试将返回真值。
##### 举例：
```
class FlyweightBase(object):
    def offer(self):
        #享元基类
        pass

class Flyweight(FlyweightBase):
   #共享享元类
    def __init__(self, name):
        self.name = name

    def get_price(self, price):
        print(f'产品类型：{self.name} 详情：{price}')

class FactoryFlyweight(object):
    #享元工厂类
    def __init__(self):
        self.product = {}

    def Getproduct(self, key):
        if not self.product.get(key, None):
            self.product[key] = Flyweight(key)
        return self.product[key]

if __name__ == '__main__':
    test = FactoryFlyweight()
    A = test.Getproduct("高端")
    A.get_price("香水：80")
    B = test.Getproduct("高端")
    B.get_price("面膜：800")
```

输出结果：
```
产品类型：高端 详情：香水：80
产品类型：高端 详情：面膜：800
```
---
layout: post
title: python设计模式之单例模式
date: 2017-04-16 18:33:21
tags:  水滴石穿
---
### 单例模式
保证一个类仅有一个实例，并提供一个访问它的全局访问点。

##### 意义：
- 当类只能有一个实例而且客户可以从一个众所周知的访问点访问它时。
- 当这个唯一实例应该是通过子类化可扩展的，并且客户应该无需更改代码就能使用一个扩展的实例时。

##### 举例：
```
class Singleton(object):

    def __new__(cls, *args, **kw):
        if not hasattr(cls, '_instance'):
            org = super(Singleton, cls)
            cls._instance = org.__new__(cls, *args, **kw)
        return cls._instance
```
复写内部方法__new__()
通过hasattr函数判断该类实例化时有没有_instance属性
如果不存在，那么继承并返回原始的__new__方法给_instance属性
如果存在则直接返回_instance属性所指的对象
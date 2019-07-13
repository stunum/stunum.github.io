---
layout: post
title: pyton标准库装饰器——property
date: 2018-01-21 01:17:05
tags:  水滴石穿
---
Python内置的 `@property` 装饰器就是负责把一个方法变成属性调用的。
举个例子：
传统的类的属性读写方式如下
```
class Student(object):
    def __init__(self,num):
        self._score=num
    def get_score(self):
        return self._score
    
    def set_score(self,num):
        self._score=num

>>>stu=Student(60)
>>>stu.get_score()
60
>>>stu.set_score(90)
>>>stu.get_score()
90
```
使用了 `@property` 装饰方法后的读写方式如下：

```
class Student(object):
    def __init__(self,num):
        self._score=num

    @property
    def score(self,num):
        return self._score
    
    @score.setter
    def score(self,num):
        self._score=num


>>> stu=Student(60)
>>> stu.score    #读取stu对象的score值
60
>>> stu.score=90 #设置score的值为90
>>> stu.score    #读取stu对象的score值
90
```
如果不设置 `setter` 方法的话，那么 `score` 就只有**只读**属性。
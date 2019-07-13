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
#函数装饰器
from functools import wraps
import time

def runtime(func):
    @wraps(func)
    def inner(*args, **kwargs):
        start_time = time.time()
        func(*args, **kwargs)
        end_time = time.time()
        print(end_time - start_time)
    return inner

@runtime
def fun():
    time.sleep(2)
    print('ok')
```

```
#类装饰器
class RunTime(object):
    def __init__(self, func):
        self._func = func

    def __call__(self, *args, **kwargs):
        start_time = time.time()
        self._func(*args, **kwargs)
        end_time = time.time()
        print(end_time - start_time)


@RunTime
def fun1(num):
    print(f'args={num}')
    time.sleep(2)
    print('ok')
```
ps：如果一个函数被多个装饰器装饰的话，装饰器的执行顺序是从内到外的。
举例：
```
@ClearFunc
@RunTime
def fun1(num):
    print(f'args={num}')
    time.sleep(2)
    print('ok')
```
上面这个函数被两个装饰器装饰了，运行的时候执行顺序是原函数先被 `RunTime` 装饰器装饰，之后装饰完的函数再被 `ClearFunc` 装饰。
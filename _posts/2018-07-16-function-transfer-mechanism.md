---
layout: post
title: python的函数参数传递机制
date: 2018-07-16 21:32:58
tags:  水滴石穿
---

>函数参数传递机制问题在本质上是调用函数（过程）和被调用函数（过程）在调用发生时进行通信的方法问题。基本的参数传递机制有两种：值传递和引用传递。
>
>值传递（passl-by-value）过程中，被调函数的形式参数作为被调函数的局部变量处理，即在堆栈中开辟了内存空间以存放由主调函数放进来的实参的值，从而成为了实参的一个副本。值传递的特点是被调函数对形式参数的任何操作都是作为局部变量进行，不会影响主调函数的实参变量的值。
>
>引用传递(pass-by-reference)过程中，被调函数的形式参数虽然也作为局部变量在堆栈中开辟了内存空间，但是这时存放的是由主调函数放进来的实参变量的地址。被调函数对形参的任何操作都被处理成间接寻址，即通过堆栈中存放的地址访问主调函数中的实参变量。正因为如此，被调函数对形参做的任何操作都影响了主调函数中的实参变量。

### 那么python的函数参数传递机制是什么呢？
在讲python的函数参数传递机制之前，应该先来复习一下 `可变对象`和`不可变对象`，这对后面深刻理解python的函数传递机制有很好的铺垫。

1. ##### 可变对象定义：
###### 该对象所指向的内存中的值可以被改变。变量（准确的说是引用）改变后，实际上是其所指的值直接发生改变，并没有发生复制行为，也没有开辟新的出地址，通俗点说就是原地改变。
   * 列表
   * 字典
   * 集合

2. ##### 不可变对象定义：
###### 该对象所指向的内存中的值不能被改变。当改变某个变量时候，由于其所指的值不能被改变，相当于把原来的值复制一份后再改变，这会开辟一个新的地址，变量再指向这个新的地址。
   * 字符串
   * 数值类型（如int、float)
   * 元组

#### 测试
##### 不可变对象：
```
def TestStr(value):
    print(f'TestStr before change id={id(value)}')
    value = value + 'abc'
    print(f'TestStr after change id={id(value)}')


def TestInt(value):
    print(f'TestInt before change id={id(value)}')
    value = value + 1
    print(f'TestStr after change id={id(value)}')


def TestFloat(value):
    print(f'TestFloat before change id={id(value)}')
    value = value + 1.1
    print(f'TestFloat after change id={id(value)}')

if __name__=="__main__":
    testStr = 'ABC'
    print(f'testStr value={testStr}')
    print(f'before in func testStr id={id(testStr)}')
    TestStr(testStr)
    print(f'testStr value={testStr}')
    print('--------------------------')
    testInt = 1
    print(f'testInt value={testInt}')
    print(f'before in func testInt id={id(testInt)}')
    TestInt(testInt)
    print(f'testInt value={testInt}')
    print('--------------------------')
    testFloat = 1.3
    print(f'testFloat value={testFloat}')
    print(f'before in func testFloat id={id(testFloat)}')
    TestFloat(testFloat)
    print(f'testFloat value={testFloat}')
```
输出结果：
```
testStr value=ABC
before in func testStr id=4501515880
TestStr before change id=4501515880
TestStr after change id=4520441872
testStr value=ABC
--------------------------
testInt value=1
before in func testInt id=4499274800
TestInt before change id=4499274800
TestStr after change id=4499274832
testInt value=1
--------------------------
testFloat value=1.3
before in func testFloat id=4504920616
TestFloat before change id=4504920616
TestFloat after change id=4528695840
testFloat value=1.3
```
从上面的结果可以看到`不可变对象`在传入函数前后`id`保持一致，但是经过操作之后`id`发生改变，经过各自对应的函数之后，再打印函数的值，发现`值`依然不变。

**所以对于`不可变对象`函数传递的是`值传递`。**

##### 可变对象：
```
def TestList(value):
    print(f'TestStr before change id={id(value)}')
    value.append(4)
    print(f'TestStr after change id={id(value)}')


def TestDict(value):
    print(f'TestInt before change id={id(value)}')
    value['new'] =11
    print(f'TestStr after change id={id(value)}')


def TestSet(value):
    print(f'TestFloat before change id={id(value)}')
    value.update({'taobao'})
    print(f'TestFloat after change id={id(value)}')

if __name__=="__main__":
    testList = [1,2,3]
    print(f'testList value={testList}')
    print(f'before in func testList id={id(testList)}')
    TestList(testList)
    print(f'testList value={testList}')
    print('--------------------------')
    testDict = {'a':1,'b':2}
    print(f'testDict value={testDict}')
    print(f'before in func testDict id={id(testDict)}')
    TestDict(testDict)
    print(f'testDict value={testDict}')
    print('--------------------------')
    testSet ={1,'Google'}
    print(f'testSet value={testSet}')
    print(f'before in func testSet id={id(testSet)}')
    TestSet(testSet)
    print(f'testSet value={testSet}')
```
结果：
```
testList value=[1, 2, 3]
before in func testList id=4396845704
TestStr before change id=4396845704
TestStr after change id=4396845704
testList value=[1, 2, 3, 4]
--------------------------
testDict value={'a': 1, 'b': 2}
before in func testDict id=4383441184
TestInt before change id=4383441184
TestStr after change id=4383441184
testDict value={'a': 1, 'b': 2, 'new': 11}
--------------------------
testSet value={'Google', 1}
before in func testSet id=4396432520
TestFloat before change id=4396432520
TestFloat after change id=4396432520
testSet value={'Google', 1, 'taobao'}
```
从上面的结果可以看到`可变对象`在传入函数前后`id`保持一致，但是经过操作之后`id`仍然保持不变，经过各自对应的函数之后，再打印函数的值，发现`值`发生改变。

**所以对于`可变对象`函数传递的是`引用传递`。**

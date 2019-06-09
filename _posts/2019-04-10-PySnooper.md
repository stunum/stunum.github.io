---
layout: post
title: python调试神器之PySnooper
date: 2019-04-10 21:32:58
tags:  水滴石穿
---

### 安装 PySnooper
```
pip install pysnooper
```
### 举个例子（来自官网）
```
import pysnooper

@pysnooper.snoop()
def number_to_bits(number):
    if number:
        bits = []
        while number:
            number, remainder = divmod(number, 2)
            bits.insert(0, remainder)
        return bits
    else:
        return [0]

number_to_bits(6)
```
##### 输出内容:
```
Starting var:.. number = 6
15:29:11.327032 call         4 def number_to_bits(number):
15:29:11.327032 line         5     if number:
15:29:11.327032 line         6         bits = []
New var:....... bits = []
15:29:11.327032 line         7         while number:
15:29:11.327032 line         8             number, remainder = divmod(number, 2)
New var:....... remainder = 0
Modified var:.. number = 3
15:29:11.327032 line         9             bits.insert(0, remainder)
Modified var:.. bits = [0]
15:29:11.327032 line         7         while number:
15:29:11.327032 line         8             number, remainder = divmod(number, 2)
Modified var:.. number = 1
Modified var:.. remainder = 1
15:29:11.327032 line         9             bits.insert(0, remainder)
Modified var:.. bits = [1, 0]
15:29:11.327032 line         7         while number:
15:29:11.327032 line         8             number, remainder = divmod(number, 2)
Modified var:.. number = 0
15:29:11.327032 line         9             bits.insert(0, remainder)
Modified var:.. bits = [1, 1, 0]
15:29:11.327032 line         7         while number:
15:29:11.327032 line        10         return bits
15:29:11.327032 return      10         return bits
Return value:.. [1, 1, 0]
```


* #### 指定输出到文件
```
@pysnooper.snoop('/my/log/file.log')
```
* #### 查看一些非局部变量的表达式的值
```
@pysnooper.snoop（watch =（' foo.bar '，' self.x [“whatever”] '））
```
* #### 展开值以查看其所有属性或列表/词典项
```
@pysnooper.snoop（watch_explode =（' foo '，' self '））
```

#### 更多到用法请自行查看官方文档
[PySnooper文档链接](https://github.com/cool-RR/PySnooper)
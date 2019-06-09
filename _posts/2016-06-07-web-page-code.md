---
layout: post
title: 检查网页编码
date: 2016-06-07 23:11:54
tags:  水滴石穿
---

#### 1、使用urllib模块的getparam方法：

```
import urllib 
#import urllib.request   (python3)
urlBj = urllib.urlopen(‘http://www.baidu.com').info()
print urlBj.getparam(‘charset’)  
#print(urlBj.getparam(‘charset’))  (python3)
```
#### 2、使用chardet模块:
```
import chardet
import urlilb
#import urllib.request(python3)
urlBj = urllib.urlopen(‘http://www.baidu.com').read()
chardit1= chardet.detect(urlBj)
print chatdit1[‘encoding’]   
#print(chatdit1[‘encoding’])    (python3)
```
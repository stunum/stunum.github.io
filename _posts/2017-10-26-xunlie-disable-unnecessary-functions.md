---
layout: post
title: 关闭MacOS下迅雷不必要的功能
date: 2017-10-26 19:51:27
tags:  奇技淫巧
---

### 关闭MacOS下迅雷不必要的功能
> 众所周知下载届的两大毒瘤：百度网盘和迅雷下载。但是两者相比还是百度网盘更不要脸一点！

迅雷下载启动之后的全部多余的功能都来自于`/Applications/Thunder.app/Contents/PlugIns/`该文件夹下的插件，所以只要针对这个文件夹操作就能关闭迅雷不必要的功能！首先想到的就是暴力方法：`直接删除文件夹下的全部文件`考虑到怕迅雷后期会做软件完整性校验啥的导致无法使用，所以直接删除`PlugIns`文件夹下的内容不是最好的办法！

思来想去，突然想到，我们不是Mac系统吗？linux/uninx系统可以直接通过修改文件权限来达到不删除但是让你无法使用的目的啊！！！

用python写了一段代码来帮我们批量执行修改权限的操作
```
import os 
files_name_deque=[]
files_name_deque.extend(os.listdir('/Applications/Thunder.app/Contents/PlugIns'))
for  item in files_name_deque:
    if os.path.splitext(item)[1]!='.xlplugin':
        continue
    try:
        os.system(f'chmod a-x /Applications/Thunder.app/Contents/PlugIns/{item}')
    except Exception as e:
        print('PlugIns--->{}-->error!   pass'.format(item))
        raise(e)
```
进入python解释器交互模式复制到终端执行或者保存为`py`文件通过`python <path>/<filename>.py`来执行都是可以了。

#### 效果展示
![禁用迅雷的多余功能](https://media.stunum.com/xunl.png)

如果想要更改回去的话，只要把
```
os.system(f'chmod a-x /Applications/Thunder.app/Contents/PlugIns/{item}')
```
改为
```
os.system(f'chmod a+x /Applications/Thunder.app/Contents/PlugIns/{item}')
```
之后在执行一边就可以了。
###### 禁用迅雷的多余功能，专心下载就好~


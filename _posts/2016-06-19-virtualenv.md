---
layout: post
title: virtualenv和virtualenvwrapper安装与使用
date: 2016-06-19 22:33:18
tags:  水滴石穿
---

## virtualenv

>在python开发中，我们可能会遇到一种情况，就是当前的项目依赖的是某一个版本，但是另一个项目依赖的是另一个版本，这样就会造成依赖冲突，而virtualenv就是解决这种情况的，virtualenv通过创建一个虚拟化的python运行环境，将我们所需的依赖安装进去的，不同项目之间相互不干扰

#### 一、安装virtualenv

```
pip install virtualenv
```
这样就安装好了pip，然后我们再使用pip安装virtualenv。

#### 二、使用virtualenv
##### 1）创建虚拟环境
```
virtualenv env27
```
此命令表示创建了一个名为env27的虚拟环境。每个虚拟环境都包含一个独立的env27/bin/python和env27/bin/pip
虚拟环境建立好之后，我们可以看到env27目录下自动生成了三个文件夹：
* /bin    #包含一些在这个虚拟环境中可用的命令，以及开启虚拟环境的脚本activate    
* /include  #包含虚拟环境中的头文件，包括Python的头文件
* /lib      #包含所有的依赖库文件

##### 2）创建指定解释器版本的虚拟环境：
```
virtualenv -p python2.7 env2.7   #解释器为python2.7
virtualenv -p python3.4 env3.4   #解释器为python3.4
```
这样我们分别就建立了两个python版本的虚拟环境

##### 3)创建继承第三方的虚拟环境

如果python已经安装了第三方库，你希望在新的虚拟环境中也使用这些库，那么可使用如下命令：
```
virtualenv --system-site-packages env27
```
如果不想使用可使用如下命令：
```
virtualenv --no-site-packages /project/env27
```
##### 4)启动虚拟环境
```
source /project/env27/bin/activate
```
这个命令会修改系统路径\$PATH，把env27/bin的路径置于系统路径之前source 命令表示更改当前的shell环境。
启动了虚拟环境之后，所有pip命令新安装的第三方库都将安装在当前环境下，而不会影响系统环境或者其它虚拟环境。

此时已经我们已经启动并进入到env27环境中了，现在我们可以安装我们需要使用的库了，比如我们想要安装request库：
```
pip install requests
```
这时候我们的env27环境中就有request库了，而且不会和其他的环境相互干扰。
##### 5)退出虚拟环境
```
deactivate
```
##### 6)删除虚拟环
如果想要删除env27虚拟环境，只要把/project/env27目录下的 bin、include 和 lib 三个目录删掉就好了。
virtualenv 的一个最大的缺点就是，每次开启虚拟环境之前要去虚拟环境所在目录下的 bin 目录下 source 一下 activate，这就需要我们记住每个虚拟环境所在的目录。

## virtualenvwrapper
为什么要使用Virtualenvwrapper？

 上节讲过virtualenv的缺点，而Virtaulenvwrapper，将所有的虚拟环境目录全都集中起来，比如放到 ~/virtualenvs/，对不同的虚拟环境使用不同的目录来管理。并且，它还省去了每次开启虚拟环境时候的 source 操作，使得虚拟环境更加好用。

Virtaulenvwrapper是virtualenv的扩展包，用于更方便管理虚拟环境，比如它可以：将所有虚拟环境整合在一个目录下、管理（新增，删除，复制）虚拟环境、快速切换虚拟环境等等。
##### 1)安装
```
pip install virtualenvwrapper
```
##### 2)配置virtualenvwrapper
安装成功后， 在/usr/local/bin/目录下会自动生成一个名为virtualenvwrapper.sh 的shell脚本文件，这个文件就是用来启动virtualenvwrapper的。

使用如下命令可以启动virtualenvwrapper：
```
source /usr/local/bin/virtualenvwrapper.sh
```
###### 配置自动启动virtualenvwrapper
每次打开终端后必须要输入这么长的命令才可以启动它。所以我们需要配置一个打开终端就自动启动。

因为virtualenvwrapper默认将所有的虚拟环境放在～/.virtualenvs目录下进行管理，所以可以修改终端的配置文件，将环境变量 WORKON_HOME 来指定为虚拟环境的保存目录。

###### 终端配置文件中操作
###### （1）vim 打开~/.bash_profile
```
vim ~/.bash_profile
```
###### （2）粘贴下面代码到~/.bash_profile文件末尾，然后保存退出。
```
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```
然后在终端中运行：
```
source ~/.bash_profile
```
完成以上配置文件的修改后，以后每次再启动终端的时候virtualenvwrapper都会自动运行了。
##### 3)使用virtualenvwrapper
###### 创建虚拟环境
```
mkvirtualenv env27
```
这样我们就创建了一个名叫env27的虚拟环境，它被存放在 $WORKON_HOME/spider 目录下
###### 创建指定解释器的虚拟环境
```
mkvirtualenv -p python2.7 env27
mkvirtualenv -p python3.4 env34
```
这样我们分别就建立了两个python版本的虚拟环境
###### 启动虚拟环境/切换虚拟环境
```
workon env27
```
可以通过以下命令查看当前python环境的版本
```
python --version
>>Python 2.7.6
```
再切换到3.4的环境
```
workon env34
```
查一下版本
```
python --version
>>Python 3.4.0
```
###### 退出虚拟环境
```
deactivate
```
###### 删除虚拟环境
```
rmvirtualenv env27
```

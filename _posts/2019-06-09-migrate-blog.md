---
layout: post
title: 迁移blog
date: 2019-06-09 20:12:25
tags: 朝花夕拾
---

毕业也一年了，又要到了一年一次的高考了，希望考生能取得自己满意的成绩吧！

阿里云的学生服务器快要到期了，趁着端午假期，提前把服务器上用wordpress搭建的博客里面的内容迁移到GitHub Pages。

选择GitHub Pages的原因：
* 免费，只要注册github之后就可以免费使用；
* 有版本管理，修改变更方便；
* 可以使用markdown；
  
相对于这些优点来说，GitHub Pages缺点对于个人blog来说真的是不重要了

我采用了 Vno-Jekyll主题的Jekyll来搭建静态的博客，把原来Wordpress中的文章改成markdown格式，都搬到GitHub Pages中，并设置解析了blog.stunum.com子域名到GitHub Pages中。

配置好这个主题到GitHub Pages在浏览器加载的时候会很慢，使用chrome的调试模式，跟踪性能的时候发现了问题：
* 引用的官网的jquery.js文件加载缓慢；
* bootstrap的css本地文件加载缓慢；
* 背景图片和头像图片过大导致加载缓慢；
  
优化：
* 图片进行大小削减；
* 使用[七牛对象存储](https://portal.qiniu.com/signup?code=1h99yr82jnnf6)，把jquery、css和图片通过外链的形式引入；
* 设置七牛的图片样式，保存原始比例压缩50%，html文件中引入的时候通过样式分隔符在尾部添加图片样式名；

由于GitHub Pages开启了https，所以七牛中也要设置https的外链，好在七牛现在提供免费的证书，可以自己去申请，然后配置https外链。


#### 恰饭链接
* [七牛云好友邀请链接](https://portal.qiniu.com/signup?code=1h99yr82jnnf6)
* [七牛云的免费ssl证书申请链接](https://portal.qiniu.com/certificate/apply)


目前只迁移了旧的blog上的文章，还有一部分写在了`印象笔记`上了，改天有空在迁移过来吧！

👋，wordpress！GitHub Pages真香！！！
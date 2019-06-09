---
layout: post
title: 搭建个人blog之环境搭建
date: 2016-05-10 09:45:29
tags:  水滴石穿
---

我选择了Wordpress来做个人blog的平台，选择这个的理由是：
* WordPress 功能强大、扩展性强，这主要得益于其插件众多，易于扩充功能，基本上一个完整网站该有的功能，通过其第三方插件都能实现所有功能；
* 适合DIY，如果你是喜欢丰富内容的网站，那么wordpress可以很好地符合你的胃口。
* 主题很多，网站上一大片都是wordpress的主题，各色各样，应有尽有！
* wordpress有强大的社区支持，有上千万的开发者贡献和审查wordpress，所以wordpress是安全并且活跃的。

接下来就讲一下整个搭建的过程吧！

### 一、申请开通云服务器
国内的云服务器首推阿里和腾讯这两家，两家都有针对学生的优惠福利！

阿里云是9.9元/月的学生套餐，~~腾讯是1元/月的学生套餐~~ 腾讯是10元/月的学生套餐
* [阿里云](https://chuangke.aliyun.com/invite?userCode=rrl6qrwb)
* [腾讯云](https://cloud.tencent.com/redirect.php?redirect=1040&cps_key=509f56cc181b86770096981b66bdb3b4&from=console)
  
云服务器的系统选择方面推荐使用CentOS系统，当然如果你也可以选择你自己熟悉的系统来使用。如果需要域名的话，也可以购买域名，解析到服务器，现在国内的域名都是要备案的！！

ps:浙江省网站的备案好像腾讯的新网没办法做

### 二、搭建环境
### Apache+MySQL+PHP：
  
#### Apache：
1、安装apache,并设置开机启动
```
[root@localhost ~]# yum install httpd
[root@localhost ~]# chkconfig --levels 35 httpd on
[root@localhost ~]# service httpd start
```
这时候可以测试apache是否正常工作,浏览器访问服务器ip会出现欢迎界面，如果不能正常显示的话，请检查服务器的安全组设置和机器本身的防火墙设置！

#### MySQL：
1.安装mysql,并设置mysql开机自启动，同时启动mysql
```
[root@localhost ~]# yum install mysql
[root@localhost ~]# yum install mysql-server
[root@localhost ~]# chkconfig --levels 35 mysqld on
[root@localhost ~]# service mysqld start
```
2.配置mysql的root密码
```
[root@localhost ~]# mysql_secure_installation
```
会出现以下选项
```

Enter current password for root (enter for none): ( 回车)
OK, successfully used password, moving on...
Setting the root password ensures that nobody can log into the MySQL
root user without the proper authorisation.
Set root password? [Y/n] Y
New password: (例：123456)
Re-enter new password: (例：123456)
Password updated successfully!
Reloading privilege tables..
 ... Success!
By default, a MySQL installation has an anonymous user, allowing anyone
to log into MySQL without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.
Remove anonymous users? [Y/n] Y
(是否移出数据库的默认帐户，如果移出，那么在终端中直接输入mysql是会提示连接错误的)
Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.
Disallow root login remotely? [Y/n] Y (是否禁止root的远程登录)
By default, MySQL comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.
Remove test database and access to it? [Y/n] Y
Reload privilege tables now? [Y/n] Y

```
#### PHP：
安装php
```
[root@localhost ~]# yum install php
[root@localhost ~]# yum install php-mysql php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc
```

#### phpMyAdmin:
安装phpMyAdmin
```
[root@localhost ~]# yum install phpmyadmin
```

安装完成后还需要配置一下访问权限，使得出了本机外，其他机子也能访问phpMyAdmin
```
vi /etc/httpd/conf.d/phpMyAdmin.conf
```
找到两个directory的权限设置，Allow from 改成All

(只显示部分需要更改的代码)
```
<Directory /usr/share/phpMyAdmin/>
   Order Deny,Allow
   Deny from All
   Allow from 127.0.0.1
   Allow from All
</Directory>
<Directory /usr/share/phpMyAdmin/setup/>
   Order Deny,Allow
   Deny from All
   Allow from 127.0.0.1
   Allow from All
</Directory>
```
重启服务器
```
[root@localhost ~]#  service httpd restart
```
测试http://(IP地址)/phpMyAdmin
用户名:root 密码:123456 (密码是之前mysql服务安装时设置的密码)
到这里为止，我们的服务环境就搭建完成。

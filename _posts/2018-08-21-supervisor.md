---
layout: post
title: supervisor守护程序
date: 2018-08-21 01:18:49
tags:  水滴石穿
---

> supervisor是用Python开发的一个client/server服务，是Linux/Unix系统下的一个进程管理工具。可以很方便的监听、启动、停止、重启一个或多个进程。用supervisor管理的进程，当一个进程意外被杀死，supervisor监听到进程死后，会自动将它重启，很方便的做到进程自动恢复的功能，不再需要自己写shell脚本来控制。

### 安装supervisor
```
$ pip install supervisor
```
### 使用supervisor
#### 1、 生成默认的配置文件

`$ echo_supervisord_conf > supervisord.conf`

#### 2、 修改刚才生成的默认配置文件

`$ vim ./supervisord.conf`

在`supervisord.conf`文件的尾部找到以下代码并修改
```
$ -- ;[include]
$ -- ;files = relative/directory/*.ini
$ ++ [include]
$ ++ files = /etc/supervisord/*.conf
```
以上修改的目的是使supervisord在加载默认配置文件的时候同时加载`/etc/supervisord/`目录下的所有以conf结尾的配置文件，对以后添加/删除进程管理来说会很方便。

#### 3、 编写需要管理的进程的配置文件
```
[program:odoo12ERP]
command=/appdata/odoo12_2/venv/bin/python3 /appdata/odoo12_2/odoo/odoo-bin -c /etc/odoo12.conf  ; the program (relative uses PATH, can take args)
process_name=%(program_name)s ; process_name expr (default %(program_name)s)
numprocs=1                    ; number of processes copies to start (def 1)
;directory=/tmp                ; directory to cwd to before exec (def no cwd)
;umask=022                     ; umask for process (default None)
priority=999                  ; the relative start priority (default 999)
autostart=true                ; start at supervisord start (default: true)
startsecs=3                   ; # of secs prog must stay up to be running (def. 1)
startretries=5                ; max # of serial start failures when starting (default 3)
autorestart=unexpected        ; when to restart if exited after running (def: unexpected)
exitcodes=0                   ; 'expected' exit codes used with autorestart (default 0)
stopsignal=QUIT               ; signal used to kill process (default TERM)
stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
;stopasgroup=false             ; send stop signal to the UNIX process group (default false)
;killasgroup=false             ; SIGKILL the UNIX process group (def false)
;user=chrism                   ; setuid to this UNIX account to run the program
;redirect_stderr=true          ; redirect proc stderr to stdout (default false)
stdout_logfile=/appdata/odoo12_2/supervisorFiles/        ; stdout log path, NONE for none; default AUTO
stdout_logfile_maxbytes=50MB   ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=10     ; # of stdout logfile backups (0 means none, default 10)
;stdout_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stdout_events_enabled=false   ; emit events on stdout writes (default false)
;stdout_syslog=false           ; send stdout to syslog with process name (default false)
;stderr_logfile=/a/path        ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10     ; # of stderr logfile backups (0 means none, default 10)
;stderr_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stderr_events_enabled=false   ; emit events on stderr writes (default false)
;stderr_syslog=false           ; send stderr to syslog with process name (default false)
;environment=A="1",B="2"       ; process environment additions (def no adds)
;serverurl=AUTO                ; override serverurl computation (childutils)
```
以上是我以`odoo`程序作为例子编写的配置。

值得注意的一点是`command`参数里面的命令必须是前台形式的，不能用后台守护模式命令，不然`supervisor`无法管理该程序的进程

#### 3、 启动supervisor

```
$ supervisord -c /etc/supervisord.conf 
```
然后就可以通过浏览器打开`http://localhost:9001`访问管理页面。

如果修改过默认配置里面的端口、登录验证等，以设置的来登录。

###### 更多设置请前往官网查看文档

[supervisor官方文档链接](http://www.supervisord.org)
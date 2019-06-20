---
layout: post
title: docker之nginx
date: 2018-12-11 00:07:39
tags: 水滴石穿
---

#### 安装 nginx

```
docker pull nginx
```

#### docker 中 nginx 的文件位置

- 日志文件 `/var/log/nginx`
- 配置文件 `/etc/nginx/conf.d`
- 项目文件 `/usr/share/nginx/html`

#### docker 命令启动 nginx

```
docker --name ngixn-server -p 80:80 -v ~/nginx/log:/var/log/nginx -v ~/nginx/www:/usr/share/nginx/html -v ~/nginx/conf:/etc/nginx/conf.d -d nginx:latest
```

#### docker-compose 启动 nginx

```
version: '3'
services:
    nginx_server:
        image: nginx:latest
        container_name: nginx-server
        ports:
            - 80:80

        volumes:
            - ~/nginx/log:/var/log/nginx
            - ~/nginx/www:/usr/share/nginx/html
            - ~/nginx/conf:/etc/nginx/conf.d
        restart: always
    #... 其他的服务
```

#### 配置文件 `nginx.conf`

```
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;
events {
    worker_connections  1024;
}
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;
    sendfile        on;
    #tcp_nopush     on;
    keepalive_timeout  65;
    #gzip  on;
    include /etc/nginx/conf.d/*.conf;
}
```

- user:运行 nginx 的用户
- worker_processes: worker_processes 数量,一般是和 cpu 核数一样，但是也有特殊情况，比如处理的都是一些费时的类似 IO 操作，那么就可以把 worker 数量设置的比核数稍微多一点，具体要根据实际业务来设置。
- error_log: 错误日志文件位置以及日志的级别，级别包含：debug info notice warn error crit alert emerg,级别依次增大。如果设置了 debug，必须在 configure 时加入--with-debug 配置
- pid: 保存 master 进程 ID 的 pid 文件存放路径。
- worker_connections: 每个 worker 进程同时处理的最大连接数。
- keepalive_timeout: 连接超时时间。
- include: 额外包含的内容。可以使用正则来做匹配。
- log_format: 格式化日志格式。

自定义的 config 文件可以放置到 include 设置的文件夹下。举个例子创建一个 nginx-uwsgi 的配置文件：

```
server {
    listen 80 default_server;
    server_name  localhost;
    charset UTF-8;

    location /static {
        alias /usr/share/nginx/html/static/;
    }

    location / {
        include uwsgi_params;
        uwsgi_pass 127.0.0.1:8009;
        uwsgi_read_timeout 10;
    }
}
```

然后 docker restart nginx-server

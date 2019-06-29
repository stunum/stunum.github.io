---
layout: post
title: Django多数据库配置
date: 2018-03-26 18:29:07
tags: 水滴石穿
---

### Django多数据库配置

### setting.py文件配置多数据库
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'xxxx', #数据库名字
        'USER': 'xxxx', #用户名，数据库的拥有者
        'PASSWORD':'xxxx',#登录密码
        'HOST':xxxx',#主机地址本地可配置localhost或127.0.0.1。前提是安装postgresql的时候配置pg_hba.conf要配置好。可查看这里。
        'PORT':'5432',#可以使用默认端口号
    },
    'other': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'xxxx', #数据库名字
        'USER': 'xxxx', #用户名，数据库的拥有者
        'PASSWORD':'xxxx',#登录密码
        'HOST':xxxx',#主机地址本地可配置localhost或127.0.0.1。前提是安装postgresql的时候配置pg_hba.conf要配置好。可查看这里。
        'PORT':'5432',#可以使用默认端口号
    }
}
```


### 定义数据库路由的类：

##### 不同的app访问不同数据库的多数据库配置
```
//此类为根据不同的app访问不同的数据库
class AuthRouter:
    """
    A router to control all database operations on models in the
    auth application.
    """
    def db_for_read(self, model, **hints):
        """
        Attempts to read auth models go to auth_db.
        """
        if model._meta.app_label == 'auth':
            return 'auth_db'
        return None

    def db_for_write(self, model, **hints):
        """
        Attempts to write auth models go to auth_db.
        """
        if model._meta.app_label == 'auth':
            return 'auth_db'
        return None

    def allow_relation(self, obj1, obj2, **hints):
        """
        Allow relations if a model in the auth app is involved.
        """
        if obj1._meta.app_label == 'auth' or \
           obj2._meta.app_label == 'auth':
           return True
        return None

    def allow_migrate(self, db, app_label, model_name=None, **hints):
        """
        Make sure the auth app only appears in the 'auth_db'
        database.
        """
        if app_label == 'auth':
            return db == 'auth_db'
        return None


//使用该路由类需把以下内容添加到setting.py文件中
DATABASE_ROUTERS =['Project.app.file.DatabaseAppsRouter']
//setting.py 文件中还需要配置app的model对应使用的数据库
DATABASE_APPS_MAPPING = {
    # app   : database alis
    # 未做指定的app会使用默认数据库
    'game' : 'other',
    'visitor':'other'
}

```

##### 数据库读写分离的多数据库配置

```
//此数据库路由类适用于做了数据库读写分离部署的情况，
//写入都是在主数据库，读随机分配从数据库
import random

class PrimaryReplicaRouter:
    def db_for_read(self, model, **hints):
        """
        Reads go to a randomly-chosen replica.
        """
        return random.choice(['replica1', 'replica2'])

    def db_for_write(self, model, **hints):
        """
        Writes always go to primary.
        """
        return 'primary'

    def allow_relation(self, obj1, obj2, **hints):
        """
        Relations between objects are allowed if both objects are
        in the primary/replica pool.
        """
        db_list = ('primary', 'replica1', 'replica2')
        if obj1._state.db in db_list and obj2._state.db in db_list:
            return True
        return None

    def allow_migrate(self, db, app_label, model_name=None, **hints):
        """
        All non-auth models end up in this pool.
        """
        return True


//使用该路由类需把以下内容添加到setting.py文件中
DATABASE_ROUTERS =['Project.app.file.DatabaseAppsRouter']
```

---
layout: post
title: rest_framework中django-filter的In查询操作
date: 2018-09-11 22:17:52
tags: 水滴石穿
---

#### IN 操作：
举个例子：

想要查询tb_student表中ID为1、3、5、6的学生的name、class、score的信息
```
//原始sql语句：
SELECT name,class,score FROM tb_student WHERE id in (1,3,5,6);
```

在使用 rest_framework 的筛选时：
```
//django_filters的写法
import django_filters

#继承BaseInFilter以及想要做IN操作的字段类型，比如NumberFilter、CharFilter等
class NumberInFilter(django_filters.BaseInFilter,django_filters.NumberFilter):
    pass

class StudentFilter(django_filters.rest_framework.FilterSet):
    ...
    ser_id=NumberInFilter(field_name='utype',lookup_expr='in')
    ...
    class Meta:
    model = Student
    fields = [...,'ser_id',...]
```
---
layout: post
title: rest_framework源码之generics
date: 2018-09-22 21:54:30
tags: 水滴石穿
---

### generics

`generics.py` 文件中的类：

- GenericAPIView
- CreateAPIView
- ListAPIView
- RetrieveAPIView
- DestroyAPIView
- UpdateAPIView
- ...

以及从`mixins.py`引入的 5 个重要的类：

- mixins.CreateModelMixin
- mixins.ListModelMixin
- mixins.RetrieveModelMixin
- mixins.UpdateModelMixin
- mixins.DestroyModelMixin

GenericAPIView 这个类继承自 APIView 所以里面的东西基本上就是使用父类的以及个别新的方法。

- `get_queryset`方法：获得 queryset 对象,可以通过定义类中`queryset`属性来更改。重写该方法，更具不同的请求来返回不同的 queryset 对象
- `get_serializer`、`get_serializer_context`和`get_serializer_class`方法：获得指定的`serializer`类，可以通过定义类中的`serializer_class`属性来更改，
- `filter_queryset`方法：过滤
- `paginator`、`paginate_queryset`和`get_paginated_response`方法：分页

`generics.py`中的其他类通过继承`GenericAPIView`类和`mixins`中的类并定义的与 http 请求对应的方法，然后返回`mixins.py`中的各个类的方法来实现对应的效果。

我们也可以自己通过继承组合的方式来定义需要的 APIView.

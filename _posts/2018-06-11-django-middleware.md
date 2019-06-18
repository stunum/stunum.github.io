---
layout: post
title: Django-middleware
date: 2018-06-11 21:33:17
tags: 水滴石穿
---

> 中间件是一个 hook 框架，它们可以介入 Django 的请求和响应处理过程。 它是一个轻量级、底层的“插件”系统，用于在全局修改 Django 的输入或输出。

### Django 的中间件

自定义中间件类可以继承 `MiddlewareMixin` 类。

```
class MiddlewareMixin:
    def __init__(self, get_response=None):
        self.get_response = get_response
        super().__init__()

    def __call__(self, request):
        response = None
        if hasattr(self, 'process_request'):
            response = self.process_request(request)
        response = response or self.get_response(request)
        if hasattr(self, 'process_response'):
            response = self.process_response(request, response)
        return response
```

`MiddlewareMixin`中的 `__call__`方法使得每个自定义中间件如果定义了 `process_request`和`process_response`的方法那么就一定会被执行！并且`process_response`方法必须有返回值，也就是要有`return`。

#### Request 预处理函数: process_request(self, request)

request 是一个 HttpRequest 对象。 view_func 是 Django 会调用的一个 Python 的函数。 （它是一个真实的函数对象，不是函数的字符名称。) view_args 是一个会被传递到视图的位置参数列表，而 view_kwargs 是一个会被传递到视图的关键字参数字典。 view_args 和 view_kwargs 都不包括第一个视图参数(request)。

process_view()会在 Django 调用视图之前被调用。它应该返回一个 None 或一个 HttpResponse 对象。 如果返回 None，Django 将会继续处理这个请求，执行其它的 process_view() 中间件，然后调用对应的视图。 如果它返回一个 HttpResponse 对象，Django 不会打扰调用相应的视图；它将应用响应中间件到 HttpResponse 并返回结果。

#### View 预处理函数: process_view(self, request, callback, callback_args,callback_kwargs)

request 是一个 HttpRequest 对象。 Exception 是一个被视图中的方法抛出来的 exception 对象。

当一个视图抛出异常时，Django 会调用 process_exception()来处理。 None 应该返回一个 process_exception() 或者一个 HttpResponse 对象。 如果它返回一个 HttpResponse 对象，则将应用模板响应和响应中间件，并将生成的响应返回给浏览器。 否则，default exception handling 开始。

再次提醒，在处理响应期间，中间件的执行顺序是倒序执行的，这包括 process_exception。 如果异常中间件返回响应，那么中间件上面的中间件类的 process_exception 方法根本就不会被调用。

#### Template 模版渲染函数：process_template_response()

request 是一个 HttpRequest 对象。 response 是一个 TemplateResponse 对象（或等价的对象），由 Django 视图或者中间件返回。

如果响应的实例有 render()方法，process_template_response()在视图刚好执行完毕之后被调用，这表明了它是一个 TemplateResponse 对象（或等价的对象）。

这个方法必须返回一个实现了 render 方法的响应对象。 它可以修改给定的 response.template_name 对象，通过修改 response 和 response.context_data 或者它可以创建一个全新的 TemplateResponse 或等价的对象。

你不需要显式渲染响应 —— 一旦所有的模板响应中间件被调用，响应会自动被渲染。

在一个响应的处理期间，中间件以相反的顺序运行，这包括 process_template_response()。

#### Exception 后处理函数:process_exception(self, request, exception)

Django 自动将视图或中间件引发的异常转换为具有错误状态代码的适当 HTTP 响应。 Certain exceptions 转换为 4xx 状态码，而将未知异常转换为 500 状态码。

#### Response 后处理函数:process_response(self, request, response)

request 是 request 对象，而 response 则是从 view 中返回的 response 对象。
这个方法的调用时机在 Django 执行 view 函数并生成 response 之后。
该处理器能修改 response 的内容；一个常见的用途是内容压缩，如 gzip 所请求的 HTML 页面。

通过重写这五个方法可以处理 django 的输入输出。

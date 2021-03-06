---
layout: post
title: rest_framework源码之APIView
date: 2018-09-19 23:15:08
tags: 水滴石穿
---

APIView 类中很最要的一个方法：`dispatch`
该方法中有两个需要注意的关键点：

- request 替换
- http 请求映射类中的对应名称的方法

```
def dispatch(self, request, *args, **kwargs):
    self.args = args
    self.kwargs = kwargs
    request = self.initialize_request(request, *args, **kwargs)
    self.request = request
    self.headers = self.default_response_headers # deprecate?

    try:
        self.initial(request, *args, **kwargs)

        # Get the appropriate handler method
        if request.method.lower() in self.http_method_names:
            handler = getattr(self, request.method.lower(),
                            self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed

        response = handler(request, *args, **kwargs)

    except Exception as exc:
        response = self.handle_exception(exc)

    self.response = self.finalize_response(request, response, *args, **kwargs)
    return self.response
```

### request 替换

以下三段代码将原本`Django`中的`request`替换成了新的`request`，如果需要使用旧的`request`则需要`request._request`来获取旧`request`。

```
#dispatch方法
request = self.initialize_request(request, *args, **kwargs)

##self.initialize_request
def initialize_request(self, request, *args, **kwargs):
    """
    Returns the initial request object.
    """
    parser_context = self.get_parser_context(request)

    return Request(
        request,
        parsers=self.get_parsers(),
        authenticators=self.get_authenticators(),
        negotiator=self.get_content_negotiator(),
        parser_context=parser_context
    )

#Request类的__init__初始化方法
def __init__(self, request, parsers=None, authenticators=None,
                negotiator=None, parser_context=None):
    assert isinstance(request, HttpRequest), (
        'The `request` argument must be an instance of '
        '`django.http.HttpRequest`, not `{}.{}`.'
        .format(request.__class__.__module__, request.__class__.__name__)
    )

    self._request = request
    self.parsers = parsers or ()
    self.authenticators = authenticators or ()
    self.negotiator = negotiator or self._default_negotiator()
    self.parser_context = parser_context
    self._data = Empty
    self._files = Empty
    self._full_data = Empty
    self._content_type = Empty
    self._stream = Empty

    if self.parser_context is None:
        self.parser_context = {}
    self.parser_context['request'] = self
    self.parser_context['encoding'] = request.encoding or settings.DEFAULT_CHARSET

    force_user = getattr(request, '_force_auth_user', None)
    force_token = getattr(request, '_force_auth_token', None)
    if force_user is not None or force_token is not None:
        forced_auth = ForcedAuthentication(force_user, force_token)
        self.authenticators = (forced_auth,)
```

### http 请求映射类中的对应名称的方法

以下代码将 http 方法映射到自己写的 `get`、`post`等方法。所以当自己写的类继承自`APIView`时，只要类中定义了与 http 方法同名的函数，那么当对应的`http`请求过来的时候，父类中的`dispatch`方法就会自动进行分发。

```
if request.method.lower() in self.http_method_names:
            handler = getattr(self, request.method.lower(),
                            self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed

        response = handler(request, *args, **kwargs)
```

具体支持那些 `http` 方法呢？可以到 `generic` 下的 `base.py` 文件下的 `View` 类中查看：

```
http_method_names = ['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']
```

举个例子：

根据 user agent 来做不同终端的页面渲染，甚至是不同的逻辑（web 和移动端逻辑不同这很正常），这种时候很容易想到重写 dispatch 方法，来做到一种通用的处理方式。

```
from django.views.generic import View as DjangoView

class View(DjangoView):

    def _get_handler(self, request):
        '''
        根据ua获取handler
        '''
        handler_name = request.method.lower()
        if handler_name in self.http_method_names:
            handler = getattr(self, handler_name)
            if not handler:
                return self.http_method_not_allowed

        user_agent = request.META.get('HTTP_USER_AGENT', '').lower()
        handler = getattr(self, handler_name, self.http_method_not_allowed)
        user_agents = ['ipad', 'iphone', 'ipod', 'androidtv', 'android']
        for ua in user_agents:
            if ua in user_agent:
                handler_name = '{}_{}'.format(handler_name, ua)
                break
        return getattr(self, handler_name) or handler

    def dispatch(self, request, *args, **kwargs):
        # Try to dispatch to the right method; if a method doesn't exist,
        # defer to the error handler. Also defer to the error handler if the
        # request method isn't on the approved list.
        # 支持user agent跳转
        # 如果实现了对应的方法，则直接使用对应ua的规则
        # 例如method=get, ua为iphone的ua，子类实现 get_iphone, 则使用get_iphone进行render
        handler = self._get_handler(request)
        self.request = request
        self.args = args
        self.kwargs = kwargs
        return handler(request, *args, **kwargs)
```

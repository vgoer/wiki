---
title: 05.Request请求
description: Request请求
published: 1
date: 2023-06-09T10:16:18.556Z
tags: thinkphp
editor: markdown
dateCreated: 2023-04-22T18:19:39.421Z
---

<center>请求对象</center>

[toc]

## request

>  如果要获取当前的请求信息，可以使用`\think\Request`类



#### 请求信息

如果要获取当前的请求信息，可以使用`\think\Request`类

```php
$request = Request::instance();
```

也可以使用助手函数

```php
$request = request();
```

> `Request::instance()  == request()`

#### 获取URL信息

```php
$request = Request::instance();
// 获取当前域名
echo 'domain: ' . $request->domain() . '<br/>';
// 获取当前入口文件
echo 'file: ' . $request->baseFile() . '<br/>';
// 获取当前URL地址 不含域名
echo 'url: ' . $request->url() . '<br/>';
// 获取包含域名的完整URL地址
echo 'url with domain: ' . $request->url(true) . '<br/>';
// 获取当前URL地址 不含QUERY_STRING
echo 'url without query: ' . $request->baseUrl() . '<br/>';
// 获取URL访问的ROOT地址
echo 'root:' . $request->root() . '<br/>';
// 获取URL访问的ROOT地址
echo 'root with domain: ' . $request->root(true) . '<br/>';
// 获取URL地址中的PATH_INFO信息
echo 'pathinfo: ' . $request->pathinfo() . '<br/>';
// 获取URL地址中的PATH_INFO信息 不含后缀
echo 'pathinfo: ' . $request->path() . '<br/>';
// 获取URL地址中的后缀信息
echo 'ext: ' . $request->ext() . '<br/>';
```



#### 设置/获取 模块/控制器/操作名称

```php
$request = Request::instance();
echo "当前模块名称是" . $request->module();
echo "当前控制器名称是" . $request->controller();
echo "当前操作名称是" . $request->action();
```



#### 获取请求参数

```php
$request = Request::instance();
echo '请求方法：' . $request->method() . '<br/>';
echo '资源类型：' . $request->type() . '<br/>';
echo '访问ip地址：' . $request->ip() . '<br/>';
echo '是否AJax请求：' . var_export($request->isAjax(), true) . '<br/>';
echo '请求参数：';
dump($request->param());
echo '请求参数：仅包含name';
dump($request->only(['name']));
echo '请求参数：排除name';
dump($request->except(['name']));
```

#### 输入变量

可以通过`Request`对象完成全局输入变量的检测、获取和安全过滤，支持包括`$_GET、$_POST、$_REQUEST、$_SERVER、$_SESSION、$_COOKIE、$_ENV`等系统变量，以及文件上传信息。



####  获取`PARAM`变量

`PARAM`变量是框架提供的用于自动识别`GET、POST`或者`PUT`请求的一种变量获取方式，是系统推荐的获取请求参数的方法，用法如下：

```php
// 获取当前请求的name变量
Request::instance()->param('name');
// 获取当前请求的所有变量（经过过滤）
Request::instance()->param();
// 获取当前请求的所有变量（原始数据）
Request::instance()->param(false);
// 获取当前请求的所有变量（包含上传文件）
Request::instance()->param(true);
```

使用助手函数实现：

```php
input('param.name');
input('param.');
或者
input('name');
input('');
```

因为`input`函数默认就采用PARAM变量读取方式。



#### 获取`GET`变量

```php
Request::instance()->get('id'); // 获取某个get变量
Request::instance()->get('name'); // 获取get变量
Request::instance()->get(); // 获取所有的get变量（经过过滤的数组）
Request::instance()->get(false); // 获取所有的get变量（原始数组）
```

或者使用内置的助手函数`input`方法实现相同的功能：

```php
input('get.id');
input('get.name');
input('get.');
```

#### 获取`POST`变量

```php
Request::instance()->post('name'); // 获取某个post变量
Request::instance()->post(); // 获取经过过滤的全部post变量
Request::instance()->post(false); // 获取全部的post原始变量
```

使用助手函数实现：

```php
input('post.name');
input('post.');
```

#### 获取`PUT`变量

```php
Request::instance()->put('name'); // 获取某个put变量
Request::instance()->put(); // 获取全部的put变量（经过过滤）
Request::instance()->put(false); // 获取全部的put原始变量
```

使用助手函数实现：

```php
input('put.name');
input('put.');
```

#### 获取`REQUEST`变量

```php
Request::instance()->request('id'); // 获取某个request变量
Request::instance()->request(); // 获取全部的request变量（经过过滤）
Request::instance()->request(false); // 获取全部的request原始变量数据
```

使用助手函数实现：

```php
input('request.id');
input('request.');
```

#### 获取`SERVER`变量

```php
Request::instance()->server('PHP_SELF'); // 获取某个server变量
Request::instance()->server(); // 获取全部的server变量
```

使用助手函数实现：

```php
input('server.PHP_SELF');
input('server.');
```

#### 获取`SESSION`变量

```php
Request::instance()->session('user_id'); // 获取某个session变量
Request::instance()->session(); // 获取全部的session变量
```

使用助手函数实现：

```php
input('session.user_id');
input('session.');
```

#### 获取`Cookie`变量

```php
Request::instance()->cookie('user_id'); // 获取某个cookie变量
Request::instance()->cookie(); // 获取全部的cookie变量
```

使用助手函数实现：

```php
input('cookie.user_id');
input('cookie.');
```

#### 请求类型

```php
// 是否为 GET 请求
if (Request::instance()->isGet()) echo "当前为 GET 请求";
// 是否为 POST 请求
if (Request::instance()->isPost()) echo "当前为 POST 请求";
// 是否为 PUT 请求
if (Request::instance()->isPut()) echo "当前为 PUT 请求";
// 是否为 DELETE 请求
if (Request::instance()->isDelete()) echo "当前为 DELETE 请求";
// 是否为 Ajax 请求
if (Request::instance()->isAjax()) echo "当前为 Ajax 请求";
// 是否为 Pjax 请求
if (Request::instance()->isPjax()) echo "当前为 Pjax 请求";
// 是否为手机访问
if (Request::instance()->isMobile()) echo "当前为手机访问";
// 是否为 HEAD 请求
```

助手函数

```php
// 是否为 GET 请求
if (request()->isGet()) echo "当前为 GET 请求";
……
```


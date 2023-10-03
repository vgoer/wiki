---
title: 03.Route路由
description: Route路由
published: 1
date: 2023-06-09T10:16:15.601Z
tags: thinkphp
editor: markdown
dateCreated: 2023-04-22T18:17:23.779Z
---

<center>路由</center>

[toc]

### Route

> 默认的`URL`地址显得有点长，下面就来说说如何通过路由简化`URL`访问。
>
> 提高安全性 都可以添加路由

```php
// 是否开启路由
'url_route_on'           => true,
// 路由使用完整匹配
'route_complete_match'   => false,
// 路由配置文件（支持配置多个）
'route_config_file'      => ['route'],
// 是否开启路由解析缓存
'route_check_cache'      => false,
// 是否强制使用路由
'url_route_must'         => false,
//强制路由必须每个设置路由规则
```

> 路由定义文件（application/route.php）

```php
规则
return[
	// 规则，admin模板index控制器,index方法
    'admin/:name' => 'admin/index/index',
];
//必须带参数的路由
以前：www.tp.com/admin/index/index/name
现在： www.tp.com/admin/name
```

> **定义路由规则后，原来的URL地址将会失效，变成非法请求。**

```php
// 两种写法
return[
	'admin/[:name]' => 'admin/index/index',
    // 参数可选，但是方法记得给默认值
];

// 2.
use think\Route;

Route::rule('admin/:name','admin/index/index');
```



完整匹配：

> 前面定义的`路由是只要以hello开头就能进行匹配`，如果需要完整匹配，可以使用下面的定义：

```php
return [
  	'admin/[:name]$'  => 'admin/index/index',
];
// 路由规则以$结尾的时候就表示当前路由规则需要完整匹配。

//http://tp5.com/hello/thinkphp/val/value // 不会匹配
```



#### 闭包定义

> 定义闭包为某些特殊的场景定义路由规则

```php
return [
    // 定义闭包
    'hello/[:name]' => function ($name) {
        return 'Hello,' . $name . '!';
    },
];
// 或
use think\Route;

Route::rule('hello/:name', function ($name) {
    return 'Hello,' . $name . '!';
});

// 访问 http://tp5.com/hello/thinkphp
```



#### URL分隔符

> 需要改变URL地址中的`pathinfo`参数分隔符

```php
// 设置pathinfo分隔符
'pathinfo_depr'          => '-',
```



#### 路由参数

> 还可以`约束路由规则的请求类型或者URL后缀之类的条件`

```php
// 由规则限制了必须是get请求，而且后缀必须是html
return [
  // 定义请求类型和后缀
'back/:name$' => ['admin/index/index', ['method' => 'get','ext'=>'html']]
];
// www.tp.com/back/goer.html  这样才能访问
```



#### 变量规则

> 定义 `复杂的路由规则定义满足不同的路由变量` 变量规则使用==正则表达式==进行定义

```php
//定义控制器
namespace app\Admin\controller;

class Time
{
    public function id($id)
    {
        return 'id是'.$id;
    }

    public function name($name)
    {
        return 'name是'.$name;
    }

    public function day($year,$month)
    {
        return '时间是'.$year.'年'.$month.'月';
    }

}
//参数要和路由规则参数命名一样
```

```php
//定义规则
return [
	    'time/day/:year/:month' =>['admin/time/day',['method'=>'get'],['year' => '\d{4}','month' => '\d{2}']],

    'time/id/:id' => ['admin/time/id',['method' => 'get'],['id'=>'\d+']],
    'time/name/:name' => ['admin/time/name',['method' => 'get'] , ['name' => '\w+']]

];
```



> 访问：
>
> http://www.abctp.com/time/day/1000/30     -->  时间是1000年30月
>
> http://www.abctp.com/time/id/999    -->  id是999
>
> http://www.abctp.com/time/name/rads  -->  name是abc 



#### 路由分组

> 规则一致的可以简化,  路由分组一定程度上可以`提高路由检测的效率`。

```php
return[
      '[time]' => [
        'id/:id' => ['admin/time/id',['method' => 'get'],['id' => '\d+']],
        'name/:name' => ['admin/time/name',['method' => 'get'] , ['name' => '\w+']],
        'day/:year/:month$' => ['admin/time/day',['method'=>'get'],['year' => '\d{4}','month' => '\d{2}']],
    ]  
];
```



#### 复杂路由

> 我们还需要对URL做一些特殊的定制

```php
//如果想访问
http://tp5.com/time/day-2015-05

//规则要修改  day-<year>-<month>
'day-<year>-<month>' => ['admin/time/day',['method'=>'get'],['year' => '\d{4}','month' => '\d{2}']],
```

`blog-<year>-<month>` 这样的非正常规范，我们需要使用<变量名>这样的变量定义方式，而不是 `:变量名`方式。



> 变量规则统一定义
>
> 在`__pattern__`中定义的变量规则我们称之为全局变量规则

```php
return [
    // 全局变量规则
    '__pattern__'         => [
        'name'  => '\w+',
        'id'    => '\d+',
        'year'  => '\d{4}',
        'month' => '\d{2}',
    ],

    'blog/:id'            => 'blog/get',
    // 定义了局部变量规则
    'blog/:name'          => ['blog/read', ['method' => 'get'], ['name' => '\w{5,}']],
    'blog-<year>-<month>' => 'blog/archive',
];
```



### URL生成 

> 定义路由规则之后，我们可以通过`Url类来方便的生成实际的URL地址（路由地址） `

```php
// 输出 blog/thinkphp
Url::build('blog/read', 'name=thinkphp');
Url::build('blog/read', ['name' => 'thinkphp']);
// 输出 blog/5
Url::build('blog/get', 'id=5');
Url::build('blog/get', ['id' => 5]);
// 输出 blog/2015/05
Url::build('blog/archive', 'year=2015&month=05');
Url::build('blog/archive', ['year' => '2015', 'month' => '05']);
```

我们还可以使用系统提供的==助手函数url==来简化

```php
url('blog/read', 'name=thinkphp');
// 等效于
Url::build('blog/read', 'name=thinkphp'
```



如果你配置了==url_html_suffix==参数的话，生成的URL地址会带上后缀，例如：

```php
'url_html_suffix'   => 'html',

// blog/thinkphp.html 
```



通常在==模板文件==中输出的话，可以使用`助手函数`，例如：

```php
{:url('blog/read', 'name=thinkphp')}
```



如果你的URL地址全部采用路由方式定义，也可以直接使用路由规则来定义URL生成，例如：

```php
url('/blog/thinkphp');
Url::build('/blog/8');
Url::build('/blog/archive/2015/05');
```

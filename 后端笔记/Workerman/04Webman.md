---
title: 04.Webman
description: Webman
published: 1
date: 2023-05-29T04:16:26.419Z
tags: workerman
editor: markdown
dateCreated: 2023-05-05T12:27:08.967Z
---

<center>webman</center>





[toc]




## Webman

> webman是一款基于[workerman](https://www.workerman.net/)开发的高性能HTTP服务框架。 [webman](webman是一款基于[workerman](https://www.workerman.net/)开发的高性能HTTP服务框架。)
>
> 比`laravel`快很多很多。 ==但很多功能和 laravel一样==



### 1. 安装

> `composer`	

```shell
composer create-project workerman/webman

# 开发调试和正式环境
php start.php start
php start.php start -d
```

> `id:8787` 访问





### 2. 命令行

> 命令行插件 ==快速开发==  和  `php artisan一样`

```shell
composer require webman/console
```

```shell
# 格式方法  php webman 命令
# 版本 
php webman version

# 路由表
php webman route:list

# 控制器
php webman make:controller admin
php webman make:controller api/user

# 模型
php webman make:model admin

# 中间件
php webman make:middleware Auth
```





### 3. 请求

> 获取请求数据

```php
public function index(Request $request)
{
    $deuault_name = 'webman';
    // 获取get请求的name，没有就用默认的数据
    $name = $request->get('name',$deuault_name);
    return response("hello $name");
}

# get
$request->get();
# post
$request->post();
# post包体
$post = $request->rawBody();
# header
$request->header();
# cookie
$request->cookie()
    
# 常用 all 集合
$request->all();
# 上传文件
$request->file();
# host
$request->host();
# 真实ip
$request->getRealIp($safe_mode=true);
```



### 4.响应

```php
# 返回数据
1. 字符串
   return response('hello webman gg');
2. json
    return json(['code' => 0, 'msg' => 'ok']);
3. view
    return view('index/view', ['name' => 'webman']);
4. xml
    $xml = <<<XML
       <?xml version='1.0' standalone='yes'?>
       <values>
           <truevalue>1</truevalue>
           <falsevalue>0</falsevalue>
       </values>
       XML;
    return xml($xml);
5. 重定向
      return redirect('/user');

6. 设置cookie
    return response('hello webman')
        ->cookie('foo', 'value');

7. 返回文件流
    return response()->file(public_path() . '/favicon.ico');
8. 下载文件(支持发送超大文件)
 	return response()->download(public_path() . '/favicon.ico', '可选的文件名.ico');
```



### 5.控制器

```php
php webman make:controller Admin
    
public function index(Request $request)
{

    return response("hello admin page");
}

public function getjson(Request $request)
{

    $params = $request->all();

    $data = [
        'code'  => 1,
        'msg'   => 'yes',
        'data'  => $params
    ];
    return json($data);
}
```

> 访问`http://127.0.0.1:8787/admin` 就默认到`index`控制器里面





### 6. 路由

> webman默认路由规则是 `http://127.0.0.1:8787/{控制器}/{动作}`。

```php
// config/route.php

Route::group('/api',function(){
    Route::group('/admin',function(){

        Route::any('/',[app\api\controller\AdminController::class,'index']);
    });

    // 设置中间件。
    Route::group('/index',function(){
        Route::any('/',[app\api\controller\IndexController::class,'index']);
    })->middleware([
        \app\middleware\StaticFile::class
    ]);

});
```





### 7. 中间件

> 中间件一般用于拦截请求或者响应。

```php

            ┌──────────────────────────────────────────────────────┐
            │                     middleware1                      │ 
            │     ┌──────────────────────────────────────────┐     │
            │     │               middleware2                │     │
            │     │     ┌──────────────────────────────┐     │     │
            │     │     │         middleware3          │     │     │        
            │     │     │     ┌──────────────────┐     │     │     │
            │     │     │     │                  │     │     │     │
 　── 请求 ───────────────────────> 控制器 ─ 响应 ───────────────────────────> 客户端
            │     │     │     │                  │     │     │     │
            │     │     │     └──────────────────┘     │     │     │
            │     │     │                              │     │     │
            │     │     └──────────────────────────────┘     │     │
            │     │                                          │     │
            │     └──────────────────────────────────────────┘     │
            │                                                      │
            └──────────────────────────────────────────────────────┘
```








### 二进制打包

> webman支持将项目打包成一个二进制文件，这使得webman无需php环境也能在linux系统运行起来。

```shell
composer require webman/console ^1.2.24

php webman build:bin
php webman build:bin 8.1
```

> 将webman.bin上传至linux服务器，执行 `./webman.bin start` 或 `./webman.bin start -d` 即可启动。



### Phar打包

> phar是PHP里类似于JAR的一种打包文件

```shell
composer require webman/console

php webman phar:pack

php webman.phar start 或 php webman.phar start -d
```
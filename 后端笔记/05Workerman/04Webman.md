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

> 中间件一般用于拦截请求或者响应。 [中间件](https://www.workerman.net/doc/webman/middleware.html)

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

```php
php webman make:middleware test

<?php
namespace app\middleware;

use Webman\MiddlewareInterface;
use Webman\Http\Response;
use Webman\Http\Request;

class Test implements MiddlewareInterface
{
    public function process(Request $request, callable $handler) : Response
    {
        echo '这里是请求穿越阶段，也就是请求处理前';

        $response = $handler($request); // 继续向洋葱芯穿越，直至执行控制器得到响应

        echo '这里是响应穿出阶段，也就是请求处理后';

        return $response;
    }
}
```

> 可以看下官方实例。



### 8. 视图

> webman默认使用的是php原生语法作为模版，在打开`opcache`后具有最好的性能。除了php原生模版，webman还提供了[Twig](https://twig.symfony.com/doc/3.x/)、 [Blade](https://learnku.com/docs/laravel/8.x/blade/9377)、 [think-template](https://www.kancloud.cn/manual/think-template/content) 模版引擎
>
> 强烈建议开启php.ini中`opcache.enable`和`opcache.enable_cli` 两个选项，以便模版引擎达到最好性能。

```shell
1763 [opcache]
1764 ; Determines if Zend OPCache is enabled
1765 opcache.enable=1
1766 
1767 ; Determines if Zend OPCache is enabled for the CLI version of PHP
1768 opcache.enable_cli=0

# 重启php服务
sudo systemctl restart php-fpm
```

> 具体看文档哈：[view](https://www.workerman.net/doc/webman/view.html)





### 9. 静态文件

> webman支持静态文件访问，静态文件都放置于`public`目录下
>
> 如果不需要静态文件支持，打开`config/static.php`将`enable`选项改成false。关闭后所有静态文件的访问会返回404。

> 静态文件中间件：
>
> webman自带一个静态文件中间件，位置`app/middleware/StaticFile.php`。
> 有时我们需要对静态文件做一些处理，例如给静态文件增加跨域http头，禁止访问以点(`.`)开头的文件可以使用这个中间件

```php
<?php
namespace support\middleware;

use Webman\MiddlewareInterface;
use Webman\Http\Response;
use Webman\Http\Request;

class StaticFile implements MiddlewareInterface
{
    public function process(Request $request, callable $next) : Response
    {
        // 禁止访问.开头的隐藏文件
        if (strpos($request->path(), '/.') !== false) {
            return response('<h1>403 forbidden</h1>', 403);
        }
        /** @var Response $response */
        $response = $next($request);
        // 增加跨域http头
        /*$response->withHeaders([
            'Access-Control-Allow-Origin'      => '*',
            'Access-Control-Allow-Credentials' => 'true',
        ]);*/
        return $response;
    }
}
```



### 10.session管理

```php
public function hello(Request $request)
{
    $name = $request->get('name');
    $session = $request->session();
    $session->set('name', $name);
    return response('hello ' . $session->get('name'));
}
```

```php
# 获取所有session
$session = $request->session();
$all = $session->all();

# 获取单个值
$session = $request->session();
$name = $session->get('name');

# 存储session
$session = $request->session();
$session->set('name', 'tom');
# 多个值
$session->put(['name' => 'tom', 'age' => 12]);

# 删除
$session = $request->session();
// 删除一项
$session->forget('name');
// 删除多项
$session->forget(['name', 'age']);

# 删除所有
$request->session()->flush();

# 判断session存在否
$session = $request->session();
$has = $session->has('name');
```

> 助手函数

```php
// 获取session实例
$session = session();
// 等价于
$session = $request->session();

// 获取某个值
$value = session('key', 'default');
// 等价与
$value = session()->get('key', 'default');
// 等价于
$value = $request->session()->get('key', 'default');

// 给session赋值
session(['key1'=>'value1', 'key2' => 'value2']);
// 相当于
session()->put(['key1'=>'value1', 'key2' => 'value2']);
// 相当于
$request->session()->put(['key1'=>'value1', 'key2' => 'value2']);
```

> session配置
>
> session配置文件在`config/session.php`

```php
<?php
/**
 * This file is part of webman.
 *
 * Licensed under The MIT License
 * For full copyright and license information, please see the MIT-LICENSE.txt
 * Redistributions of files must retain the above copyright notice.
 *
 * @author    walkor<walkor@workerman.net>
 * @copyright walkor<walkor@workerman.net>
 * @link      http://www.workerman.net/
 * @license   http://www.opensource.org/licenses/mit-license.php MIT License
 */

use Webman\Session\FileSessionHandler;
use Webman\Session\RedisSessionHandler;
use Webman\Session\RedisClusterSessionHandler;

return [

    'type' => 'file', // or redis or redis_cluster

    'handler' => FileSessionHandler::class,

    'config' => [
        'file' => [
            'save_path' => runtime_path() . '/sessions',
        ],
        'redis' => [
            'host' => '127.0.0.1',
            'port' => 6379,
            'auth' => '',
            'timeout' => 2,
            'database' => '',
            'prefix' => 'redis_session_',
        ],
        'redis_cluster' => [
            'host' => ['127.0.0.1:7000', '127.0.0.1:7001', '127.0.0.1:7001'],
            'timeout' => 2,
            'auth' => '',
            'prefix' => 'redis_session_',
        ]
    ],
    'session_name' => 'PHPSID',        // 存储session_id的cookie名
    'auto_update_timestamp' => false,  // 是否自动刷新session，默认关闭
    'lifetime' => 7*24*60*60,          // session过期时间
    'cookie_lifetime' => 365*24*60*60, // 存储session_id的cookie过期时间
    'cookie_path' => '/',              // 存储session_id的cookie路径
    'domain' => '',                    // 存储session_id的cookie域名
    'http_only' => true,               // 是否开启httpOnly，默认开启
    'secure' => false,                 // 仅在https下开启session，默认关闭
    'same_site' => '',                 // 用于防止CSRF攻击和用户追踪，可选值strict/lax/none
    'gc_probability' => [1, 1000],     // 回收session的几率

];
```



### 11.异常处理

> webman中异常默认由 `support\exception\Handler` 类来处理。可修改配置文件`config/exception.php`来更改默认异常处理类。异常处理类必须实现`Webman\Exception\ExceptionHandlerInterface` 接口。

```php
interface ExceptionHandlerInterface
{
    /**
     * 记录日志
     * @param Throwable $e
     * @return mixed
     */
    public function report(Throwable $e);

    /**
     * 渲染返回
     * @param Request $request
     * @param Throwable $e
     * @return Response
     */
    public function render(Request $request, Throwable $e) : Response;
}
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
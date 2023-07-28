<center>Http日志</center>



[toc]







### HTTP日志

> 获取http请求和返回的日志







### 1. 添加中间件

> 所以我是使用中间件来做的：

```shell
php artisan make:middleware AccessLog
```

> 添加代码

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Log;

class AccessLog
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request $request
     * @param  \Closure(\Illuminate\Http\Request): (\Illuminate\Http\Response|\Illuminate\Http\RedirectResponse)  $next
     * @return \Illuminate\Http\Response|\Illuminate\Http\RedirectResponse
     */
    public function handle(Request $request, Closure $next)
    {
        $traceId = md5(time() . mt_rand(1, 1000000));
        // 记录请求信息
        $requestMessage = [
            'traceId' => $traceId,
            'url' => $request->url(),
            'method' => $request->method(),
            'ip' => $request->ips(),
            'headers' => $request->header('Authorization'),
            'params' => $request->all()
        ];
        
        // 写入到 /storage/log里面
        Log::info("请求信息：", $requestMessage);

        //  打印到控制台
        Log::channel('single')->info("请求信息：", $requestMessage);

        $respone = $next($request);
        $responeData = [
            'traceId' => $traceId,
            'respone' => json_decode($respone->getContent(), true) ?? ""
        ];

        Log::info("返回信息：", $responeData);

        return $respone;
    }
}
```





### 2. 配置全局路由

> 在 app\Http\Kernel.php 里的 $middleware 数组加入 AccessLog：

```php
protected $middleware = [
    \App\Http\Middleware\TrustProxies::class,
    \Fruitcake\Cors\HandleCors::class,
    \App\Http\Middleware\PreventRequestsDuringMaintenance::class,
    \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
    \App\Http\Middleware\TrimStrings::class,
	
    \App\Http\Middleware\AccessLog::class, //全局请求返回日志记录
];

```

> 这样所有请求都会进入这个 AccessLog 里面。





### 3. 使用

```shell
public function login(Request $request)
{

    $params = $request->all();

    return response()->json([
    'code'   => 200,
    'msg'    => '获取成功',
    'data'   => $params,
    ]);
}


http://127.0.0.1:999/api/admin/login?id=100&name=goer
```





### 4. 自定义日志目录

> 1. 打开 `config/logging.php` 文件。
> 2. 在 `'channels'` 数组中添加一个新的通道，例如 `'request'`，并设置其为输出到文件。

```php
'request' => [
    'driver' => 'single',
    'path' => storage_path('logs/request.log'),
    'level' => 'info',
],
```

> 3.  使用 `Log::channel('request')->info()`

> 4. 我去，牛皮。芜湖起飞


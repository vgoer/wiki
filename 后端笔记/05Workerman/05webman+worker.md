<center>在线人数统计</center>







[toc]







## 实现多人数据统计

>  webman + GatewayWorker 简单实现在线人数统计 [blog](https://blog.csdn.net/weixin_43743720/article/details/128679566)
>
> 博客已经记载的很详细了



## 一、创建webman项目

**1、创建项目**

```shell
composer create-project workerman/webman
```



**2、给webman安装GatewayWorker插件**

（1）进入webman目录

```shell
cd webman
```

（2）执行

```shell
composer require webman/gateway-worker
```

**3、配置及业务目录**

配置文件在 `config/plugin/webman/gateway-worker/`目录
业务目录在 `plugin/webman/gateway`目录



**4、安装 redis 扩展**

> 需要下载redis。去看下以前的文章

（1）安装

```shell
composer require psr/container ^1.1.1 illuminate/redis illuminate/events
```

（2）配置
[redis配置文件](https://so.csdn.net/so/search?q=redis配置文件&spm=1001.2101.3001.7020)在 `config/redis.php`

```php
return [
    'default' => [
        'host'     => '127.0.0.1',
        'password' => null,
        'port'     => 6379,
        'database' => 0,
    ]
];
```

```php
# 多个数据库
return [
    'default' => [
        'host'     => '127.0.0.1',
        'password' => null,
        'port'     => 6379,
        'database' => 0,
    ],

    'cache' => [
        'host'     => '127.0.0.1',
        'password' => null,
        'port'     => 6379,
        'database' => 1,
    ],

]
```

```php
public function addUser()
{
    $redis = Redis::connection('cache');
    $data = range(1,10000);

    foreach($data as $k=>$v){
        $redis->set($k,rand());
    }

    return json(['code' => 200, 'msg' => 'yes']);
}
```



## 二、创建控制器

例如：
在 `app/controller/`目录下创建 `IndexController`控制器文件(webman默认创建)，代码如下：

```php
<?php

namespace app\controller;

use GatewayWorker\Lib\Gateway;

class IndexController
{
	/**
	 * 绑定关系：
     * 将client_id与uid绑定，以便通过Gateway::sendToUid($uid)发送数据，
     * 通过Gateway::isUidOnline($uid)用户是否在线。
     * uid解释：这里uid泛指用户id或者设备id，用来唯一确定一个客户端用户或者设备。
     */
    public function bind()
    {
        $clientId = "7f00000108fc00000001";
        $data = [
            'type' => 'login',
            'uid' => 1,
        ];
        Gateway::bindUid($clientId, $data['uid']);
        Gateway::sendToClient($clientId, '已成功绑定');
    }

	/**
	 * 查看用户是否在线
	 * isOnline()方法：在线返回1，不在线返回0
     */
    public function online()
    {
        if (Gateway::isUidOnline(1)) {
            print_r('在线');
        } else {
            print_r('不在线');
        }
    }
}


```



## 三、修改Events类

修改 `plugin/webman/gateway`目录中的 `Events.php`代码如下：



```php
<?php

namespace plugin\webman\gateway;

use GatewayWorker\Lib\Gateway;
use support\Redis;

class Events
{
	
    public static function onWorkerStart($worker)
    {
    
    }

	// 连接上gateway进程时触发的回调函数
    public static function onConnect($client_id)
    {
    	// 在线人数 +1
        Redis::incr('user_online_number:');
        // 向当前客户端连接发送消息，如果前端收到了表示连接成功了
        Gateway::sendToCurrentClient("Your client_id is $client_id");
    }

    public static function onWebSocketConnect($client_id, $data)
    {

    }

    public static function onMessage($client_id, $message)
    {

    }

	// 客户端与Gateway进程的连接断开时触发
    public static function onClose($client_id)
    {
    	// 在线人数 -1
        Redis::decr('user_online_number:');
    }

}

```

**以上步骤完成后，我们运行一下：**

进入`webman`目录

**（1）`debug`方式运行（用于开发调试）**

```shell
php start.php start
```

**（2）daemon方式运行（用于正式环境）**

```shell
php start.php start -d
```

**（3）windows用户：双击windows.bat 或者运行 php windows.php 启动**



## 四、创建 HTML

**1、创建 html 文件**

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <script>
        ws = new WebSocket("ws://127.0.0.1:7272");
        ws.onopen = function () {
            console.log("连接成功")
            ws.send("你好")
        }
        ws.onmessage = function(e){
            console.log("收到一条新消息：" + e.data)
        };
        ws.onclose = function(){
            console.log("已关闭")
        };
    </script>
</body>
</html>
```





> 我去，牛皮， 多个浏览页。 

> 然后打包成二进制程序。 我去，牛皮


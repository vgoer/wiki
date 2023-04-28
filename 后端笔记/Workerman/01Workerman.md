---
title: 01,Workerman
description: Workerman
published: 1
date: 2023-04-28T11:28:41.060Z
tags: workerman
editor: markdown
dateCreated: 2023-04-28T11:28:37.990Z
---

<center>Workerman</center>



[toc]



## workerman

> workerman是一款开源高性能PHP应用容器。 [workman](https://www.workerman.net/)
>
> 它大大突破了传统PHP应用范围，被广泛的用于互联网、即时通讯、APP开发、硬件通讯、智能家居、物联网等领域的开发。



### 1. 概念

> 短连接：每次都是一个新的连接。请求数据后就关闭连接了。 http等
>
> 长连接：需要不断地发送数据，连接需一直保持。 聊天，直播，游戏等场景





### 2.安装

> 安装

```shell
composer require workerman/workerman
```

#### a.Http服务

> http服务：新建文件`http_server.php`

```php
<?php

use Workerman\Connection\TcpConnection;
use Workerman\Protocols\Http\Request;
use Workerman\Worker;

require_once __DIR__ . '/vendor/autoload.php';


// 创建一个Worker监听2345端口，使用http协议通讯
$http_worker = new Worker("http://0.0.0.0:9999");

// 启动4个进程对外提供服务
$http_worker->count = 10;

// 接收到浏览器发送的数据时回复hello world给浏览器
$http_worker->onMessage = function(TcpConnection $con, Request $request)
{
    // 向浏览器发送hello world
    $con->send('hello http server');

};

// 运行worker
Worker::runAll();
```



#### b.WebSocket服务

> WebSocket 支持长连接的服务

```php
<?php
use Workerman\Connection\TcpConnection;
use Workerman\Worker;

require_once __DIR__ . '/vendor/autoload.php';

$ws_worker = new Worker("websocket://127.0.0.1:9991");
$ws_worker->count = 4;

$ws_worker->onMessage = function(TcpConnection $con, $data)
{

    $con->send("hello ws".$data);
};

Worker::runAll();
```

> 打开浏览器的控制台或者把下面代码放到html的js里面

```js
// 假设服务端ip为127.0.0.1
ws = new WebSocket("ws://127.0.0.1:9991");
ws.onopen = function() {
    alert("连接成功");
    ws.send('tom');
    alert("给服务端发送一个字符串：tom");
};
ws.onmessage = function(e) {
    alert("收到服务端的消息：" + e.data);
};
```



#### c.Tcp服务

> tcp服务

```php
<?php
use Workerman\Connection\TcpConnection;
use Workerman\Worker;

require_once __DIR__ . '/vendor/autoload.php';
$tcp_worker = new Worker("tcp://0.0.0.0:9992");

$tcp_worker->count = 4;

$tcp_worker->onMessage = function(TcpConnection $con,$data)
{
    $con->send('hello'.$data);
};
Worker::runAll();
```

> 启动`php tcp_server start` `linux命令中测试下`
>
> windows需打开 `telnet功能` 

```shell
telnet 127.0.0.1 9992
```





### 2. 直播功能

> 以前是轮询

```shell
1. 轮询： setinterval
   前端一直发送请求到后端，弊端：无论后端有无数据，前端都会一直请求。造成服务器并发量很高
   
   
2. WebSocket
   服务端和客户端建立长连接，服务端有数据会主动推送给客户端。
```

```php
# 服务端  
require_once __DIR__ . '/vendor/autoload.php';

$ws_worker = new Worker("websocket://127.0.0.1:9991");
$ws_worker->count = 4;

$ws_worker->onMessage = function(TcpConnection $con, $data)
{
    $con->send("服务端说:".$data);
};
Worker::runAll();
```

```html
# 客户端
<input type="text" name="user" id="text">
<input type="submit" value="submit" id="sub">
<script>
    let text = document.getElementById('text');
    let sub  = document.getElementById('sub');

    // 新建
    const ws = new WebSocket("ws://127.0.0.1:9991");

    sub.addEventListener('click',() => {
        let value = text.value;
        // 发送
        ws.send(value)
        text.value = ''
        return false
    })

    // 接受
    ws.onmessage = (e) => {
        console.log(e)
        alert(e.data)
    }
</script>
```










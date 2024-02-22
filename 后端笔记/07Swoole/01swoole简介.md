<center>Swoole简介</center>





[toc]







## Swoole简介

>我们先来看看 Swoole 是什么。[bilibili](https://www.bilibili.com/video/BV1m14y1H7Jz)  [swoole](https://www.swoole.com/)

> Swoole 使 PHP 开发人员可以编写高性能高并发的 TCP、UDP、Unix Socket、HTTP、 WebSocket 等服务，让 PHP 不再局限于 Web 领域。Swoole4 协程的成熟将 PHP 带入了前所未有的时期， 为性能的提升提供了独一无二的可能性。Swoole 可以广泛应用于互联网、移动通信、云计算、 网络游戏、物联网（IOT）、车联网、智能家居等领域。使用 PHP + Swoole 可以使企业 IT 研发团队的效率大大提升，更加专注于开发创新产品。

> 特点： 上述内容只是基于我自己的理解，不代表完全正确，但是大方向应该是没有问题的。想必说到这里，你也能猜到 Swoole 是如何来解决效率性能问题的。它就是通过直接将代码加载到内存的方式，就像 Java 他们一样来启动一个进程，实现 PHP 代码的高性能执行。同时，尽量保持代码还是可以按照传统的方式来写，为我们 PHP 程序员提供了一个高性能的解决方案。









### 1. 安装运行

> 请使用docker安装。查看hyper的安装教程。

```shell
# docker安装

#查看扩展
php --ri swoole
```







### 2. 启动服务

> 什么？直接起一个 Http 服务？不是要用 Nginx 或者 Apache 吗？忘了上篇文章中我们说过的东西了吧，Swoole 是直接启动服务的，而不是像传统的 PHP 使用 FastCGI 来启动 php-fpm 这种形式的。
>
> 或者，你也可以理解为直接使用 Swoole 启动的服务就是 php-fpm 使用连接形式启动的那个 9000 端口的服务。

```php
<?php
$http = new Swoole\Http\Server('0.0.0.0', 9501);

$http->on('Request', function ($request, $response) {
    echo "接收到了请求";
    $response->header('Content-Type', 'text/html; charset=utf-8');
    $response->end('<h1>Hello Swoole. #' . rand(1000, 9999) . '</h1>');
});

echo "服务启动";
$http->start();
```

> `php index.php ` 访问浏览器。 就看到服务了。




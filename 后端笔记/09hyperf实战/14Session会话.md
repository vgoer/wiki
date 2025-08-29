<center>Session会话</center>





[toc]







## Session会话

> HTTP 是一种无状态协议，即服务器不保留与客户交易时的任何状态，所以当我们在开发 HTTP Server 应用时，我们通常会通过 Session 来实现多个请求之间用户数据的共享。您可通过 [hyperf/session](https://github.com/hyperf/session) 来实现 Session 的功能。Session 组件当前仅适配了两种储存驱动，分别为 `文件` 和 `Redis`，默认为 `文件` 驱动，在生产环境下，我们强烈建议您使用 `Redis` 来作为储存驱动，这样性能更好也更符合集群架构下的使用。







### 1. 安装

```php
# 安装
composer require hyperf/session
    
# 发布配置
php bin/hyperf.php vendor:publish hyperf/session
```





## [配置 Session 中间件](https://hyperf.wiki/3.1/#/zh-cn/session?id=配置-session-中间件)

在使用 Session 之前，您需要将 `Hyperf\Session\Middleware\SessionMiddleware` 中间件配置为 HTTP Server 的全局中间件，这样组件才能介入到请求流程进行对应的处理，`config/autoload/middlewares.php` 配置文件示例如下：

```php
<?php

return [
    // 这里的 http 对应默认的 server name，如您需要在其它 server 上使用 Session，需要对应的配置全局中间件
    'http' => [
        \Hyperf\Session\Middleware\SessionMiddleware::class,
    ],
];
```







### [使用文件储存驱动](https://hyperf.wiki/3.1/#/zh-cn/session?id=使用文件储存驱动)

> 文件储存驱动是默认的储存驱动，但建议生产环境下使用 Redis 驱动

当 `handler` 的值为 `Hyperf\Session\Handler\FileHandler` 时则表明使用 `文件` 储存驱动，所有的 Session 数据文件都会被生成并储存在 `options.path` 配置值对应的文件夹中，默认配置的文件夹为根目录下的 `runtime/session` 文件夹内。

```php
// 使用
    #[Inject]
    private SessionInterface $session;

    public function getUserInfo(RequestInterface $request, ResponseInterface $response) : Psr7ResponseInterface
    {
        
        $this->session->set('foo', 'bar');
        $foo = $this->session->get('foo');
        return $response->json(["code" => 200, "message" =>  $foo]);
    }
```

```shell
a:3:{s:3:"foo";s:3:"bar";s:9:"_previous";a:1:{s:3:"url";s:33:"http://127.0.0.1:9501/favicon.ico";}s:6:"_flash";a:2:{s:3:"old";a:0:{}s:3:"new";a:0:{}}}
```

在使用 `Redis` 储存驱动之前，您需要安装 [hyperf/redis](https://github.com/hyperf/redis) 组件。当 `handler` 的值为 `Hyperf\Session\Handler\RedisHandler` 时则表明使用 `Redis` 储存驱动。您可以通过配置 `options.connection` 配置值来调整驱动要使用的 `Redis` 连接，这里的连接与 [hyperf/redis](https://github.com/hyperf/redis) 组件的 `config/autoload/redis.php` 配置内的 key 命名匹配，



### 2. 方法

```php
1. 储存数据
    
$this->session->set('foo', 'bar');

2. 获取数据
$this->session->get('foo', $default = null);

3. 获取所有数据
$data = $this->session->all();

4. 判断 Session 中是否存在某个值
if ($this->session->has('foo')) {
    //
}

5. 获取并删除一条数据
$data = $this->session->remove('foo');

6. 清空当前 Session 数据
$this->session->clear();


7. 获取当前的 Session ID
$sessionId = $this->session->getId();
```


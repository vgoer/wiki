<center>Rpc微服务</center>



[toc]







## Rpc微服务

> 微服务： 通过rpc协议，可以调用方像调用本地函数一样调用远端的函数。







### 1. 拉取hyperf

```shell
docker run --name hyperf \
-v ./skeleton:/data/project \
-p 9501:9501 -p 9502:9502 -it \
--privileged -u root \
--entrypoint /bin/sh \
hyperf/hyperf:8.1-alpine-v3.18-swoole

# 安装
cd /data/project
composer create-project hyperf/hyperf-skeleton server_rpc

cd server_rpc
php bin/hyperf.php start

# 安装watch
composer require hyperf/watcher

# 生成配置文件
php bin/hyperf.php vendor:publish hyperf/watcher

# 热更新
php bin/hyperf.php server:watch


# 设置权限
sudo chown -R 1001:1001 ./skeleton
sudo chmod -R 777 ./skeleton
```

> 服务端安装rcp [wiki](https://hyperf.wiki/3.1/#/zh-cn/json-rpc)

```shell
composer require hyperf/json-rpc

composer require hyperf/rpc-server -vvv
```

> 1. 定义服务者

```php
// app/UserService.php
<?php

namespace App\JsonRpc;

use Hyperf\RpcServer\Annotation\RpcService;

/**
 * 注意，如希望通过服务中心来管理服务，需在注解内增加 publishTo 属性
 */
#[RpcService(name: "UserService", server: "jsonrpc-http", protocol: "jsonrpc-http", publishTo: "nacos")]
class UserService implements UserServiceInterface
{
    // 实现一个加法方法，这里简单的认为参数都是 int 类型
    public function add(int $a, int $b): int
    {
        // 这里是服务方法的具体实现
        return $a + $b;
    }
}
```

```php
// 接口
// app/UserServiceInterface.php
<?php

namespace App\JsonRpc;

interface UserServiceInterface
{
    public function add(int $a, int $b): int;
}

```

> 2. 定义json-rpc server
>
> HTTP Server (适配 `jsonrpc-http` 协议)

```php
// config/autoload/server.php
<?php

use Hyperf\Server\Server;
use Hyperf\Server\Event;

return [
    // 这里省略了该文件的其它配置
    'servers' => [
        [
            'name' => 'jsonrpc-http',
            'type' => Server::SERVER_HTTP,
            'host' => '0.0.0.0',
            'port' => 9504,
            'sock_type' => SWOOLE_SOCK_TCP,
            'callbacks' => [
                Event::ON_REQUEST => [\Hyperf\JsonRpc\HttpServer::class, 'onRequest'],
            ],
        ],
    ],
];

```

> 3. 发布到服务中心: 服务中心用的是nacos

```shell
composer require hyperf/service-governance-nacos -vvv
```

> 然后再在 `config/autoload/services.php` 配置文件内配置 `drivers.nacos` 配置即可，示例如下：

```php
<?php
return [
    'enable' => [
        'discovery' => true,
        'register' => true,
    ],
    'consumers' => [],
    'providers' => [],
    'drivers' => [
        'nacos' => [
            // nacos server url like https://nacos.hyperf.io, Priority is higher than host:port
            // 'url' => '',
            // The nacos host info
            'host' => '172.24.106.239',
            'port' => 8849,
            // The nacos account info
            'username' => "nacos",
            'password' => "nacos",
            'heartbeat' => 5,
        ],
    ],
];

```

> 配置完成后，在启动服务时，Hyperf 会自动地将 `#[RpcService]` 定义了 `publishTo` 属性为 `consul` 或 `nacos` 的服务注册到对应的服务中心去。



### 2. nacos服务中心

> 安装服务中心

```shell
mkdir -p ./nacos/data ./nacos/logs

sudo chown -R 1000:1000 ./nacos
sudo chmod -R 777 ./nacos

docker pull nacos/nacos-server:latest

docker run -d --name nacos-standalone \
  -e MODE=standalone \
  -e NACOS_AUTH_TOKEN=Q2FzZV9Zb3VfTmVlZF9Bbl9FeHRyYV9Mb25nX1NlY3JldA== \
  -e NACOS_AUTH_IDENTITY_KEY=serverIdentity \
  -e NACOS_AUTH_IDENTITY_VALUE=security \
  -p 8848:8848 \
  -p 8080:8080 \
  -v ./nacos/data:/home/nacos/data \
  -v ./nacos/logs:/home/nacos/logs \
  nacos/nacos-server:latest
```

> 访问： `ip:8080`

```shell
# 博主
docker pull nacos/nacos-server:2.0.2

docker run --name nacos-quick -e MODE=standalone -p 8849:8848 -d nacos/nacos-server:2.0.2
```

> 访问： http://127.0.0.1:8849/nacos/index.html
>
> 账号密码： nacos/nacos





### 3. 创建消费者

> 创建消费者

```shell
composer create-project hyperf/hyperf-skeleton rpc_consume

# rpc组件
composer require hyperf/json-rpc
composer require hyperf/rpc-client

# 组件
composer require hyperf/service-governance-nacos
```

> 1. 创建接口

```php
// 接口
// app/UserServiceInterface.php
<?php

namespace App\JsonRpc;

interface UserServiceInterface
{
    public function add(int $a, int $b): int;
}

```

> 2. 您可通过在 `config/autoload/services.php` 配置文件内进行一些简单的配置，即可通过动态代理自动创建消费者类。

```php
<?php
return [
    // 此处省略了其它同层级的配置
    'consumers' => [
        [
            // name 需与服务提供者的 name 属性相同
            'name' => 'CalculatorService',
            // 服务接口名，可选，默认值等于 name 配置的值，如果 name 直接定义为接口类则可忽略此行配置，如 name 为字符串则需要配置 service 对应到接口类
            'service' => \App\JsonRpc\CalculatorServiceInterface::class,
            // 对应容器对象 ID，可选，默认值等于 service 配置的值，用来定义依赖注入的 key
            'id' => \App\JsonRpc\CalculatorServiceInterface::class,
            // 服务提供者的服务协议，可选，默认值为 jsonrpc-http
            // 可选 jsonrpc-http jsonrpc jsonrpc-tcp-length-check
            'protocol' => 'jsonrpc-http',
            // 负载均衡算法，可选，默认值为 random
            'load_balancer' => 'random',
            // 这个消费者要从哪个服务中心获取节点信息，如不配置则不会从服务中心获取节点信息
            'registry' => [
                'protocol' => 'consul',
                'address' => 'http://127.0.0.1:8500',
            ],
            // 如果没有指定上面的 registry 配置，即为直接对指定的节点进行消费，通过下面的 nodes 参数来配置服务提供者的节点信息
            'nodes' => [
                ['host' => '127.0.0.1', 'port' => 9504],
            ],
            // 配置项，会影响到 Packer 和 Transporter
            'options' => [
                'connect_timeout' => 5.0,
                'recv_timeout' => 5.0,
                'settings' => [
                    // 根据协议不同，区分配置
                    'open_eof_split' => true,
                    'package_eof' => "\r\n",
                    // 'open_length_check' => true,
                    // 'package_length_type' => 'N',
                    // 'package_length_offset' => 0,
                    // 'package_body_offset' => 4,
                ],
                // 重试次数，默认值为 2，收包超时不进行重试。暂只支持 JsonRpcPoolTransporter
                'retry_count' => 2,
                // 重试间隔，毫秒
                'retry_interval' => 100,
                // 使用多路复用 RPC 时的心跳间隔，null 为不触发心跳
                'heartbeat' => 30,
                // 当使用 JsonRpcPoolTransporter 时会用到以下配置
                'pool' => [
                    'min_connections' => 1,
                    'max_connections' => 32,
                    'connect_timeout' => 10.0,
                    'wait_timeout' => 3.0,
                    'heartbeat' => -1,
                    'max_idle_time' => 60.0,
                ],
            ],
        ]
    ],
];

```

> 修改消费者端http端口为`9502`

```php
// IndexController.php
<?php

declare(strict_types=1);
/**
 * This file is part of Hyperf.
 *
 * @link     https://www.hyperf.io
 * @document https://hyperf.wiki
 * @contact  group@hyperf.io
 * @license  https://github.com/hyperf/hyperf/blob/master/LICENSE
 */

namespace App\Controller;

use App\JsonRpc\UserServiceInterface;
use Hyperf\Di\Annotation\Inject;

class IndexController extends AbstractController
{

    /**
     * @Inject()
     * @var UserServiceInterface
     */
    private $userService;

    public function index()
    {
        // 订阅消费这个服务
        $sum = $this->userService->add(1,1);

        var_dump($sum);

    }
}

```




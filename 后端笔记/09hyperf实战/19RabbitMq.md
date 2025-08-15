<center>RabbitMq</center>



[toc]







## RabbitMq

> [hyperf/amqp](https://github.com/hyperf/amqp) 是实现 AMQP 标准的组件，主要适用于对 RabbitMQ 的使用。 [bili](https://www.bilibili.com/video/BV1de4y1E7Ya/?)







### 1. 安装

```bash
composer require hyperf/amqp
composer require hyperf/command
```







### 2. docker镜像

> [blog](https://blog.csdn.net/Li_WenZhang/article/details/141181632)

```shell
# 拉取，运行
docker pull rabbitmq:management

# 最新版
docker run -id --name=rabbitmq -v ./rabbitmq:/var/lib/rabbitmq -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin rabbitmq:management

# 查看启动日志
docker logs -f rabbitmq

# 数据备份 和 恢复
docker exec rabbitmq tar czf /backup/rabbitmq_backup.tar.gz /var/lib/rabbitmq
docker exec rabbitmq tar xzf /backup/rabbitmq_backup.tar.gz -C /
```

RabbitMQ 容器通过指定环境变量的方式进行配置，这比修改配置文件便捷得多。以下是一些常用的环境变量：

```shell
RABBITMQ_DEFAULT_USER：默认用户名。
RABBITMQ_DEFAULT_PASS：默认密码。
RABBITMQ_ERLANG_COOKIE：Erlang 集群 cookie。
RABBITMQ_NODENAME：节点名称。
```

更多环境变量的详细信息可以参考 RabbitMQ 官方文档 [doc](https://www.rabbitmq.com/docs/configure)。



10. 集群配置
RabbitMQ 支持集群配置，可以通过以下步骤实现：

> 阿里云：[blog](https://developer.aliyun.com/article/988115)

启动多个 RabbitMQ 容器，并确保它们可以相互通信。
在每个节点上设置相同的 RABBITMQ_ERLANG_COOKIE。
使用 rabbitmqctl 命令将节点加入集群：

```shell
docker run -d --hostname rabbit1 --name myrabbit1 -p 15672:15672 -p 5672:5672 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:management

docker run -d --hostname rabbit2 --name myrabbit2 -p 15673:15672 -p 5673:5672 --link myrabbit1:rabbit1 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:management

docker run -d --hostname rabbit3 --name myrabbit3 -p 15674:15672 -p 5674:5672 --link myrabbit1:rabbit1 --link myrabbit2:rabbit2 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' rabbitmq:management
```

> - 两个从机加入集群

```shell
docker exec -it myrabbit1 bash
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl start_app

 
docker exec -it myrabbit2 bash
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl join_cluster  rabbit@rabbit1
rabbitmqctl start_app

 
 
docker exec -it myrabbit3 bash
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl join_cluster  rabbit@rabbit1
rabbitmqctl start_app

```

- 测试普通集群是否搭建成功

进入控制台web界面查看`http://ip:15672`, 可以发现普通集群搭建成功



> 访问： `ip:15672` 默认账户：guest/guest

1. 添加用户

![image-20250619102911989](.\assets\image-20250619102911989-17503001543221.png)

2. 添加用户的虚拟机

![image-20250619103006363](.\assets\image-20250619103006363.png)

3. 添加交换机

![image-20250619103136722](.\assets\image-20250619103136722.png)

4. 添加测试队列

![image-20250619103257175](.\assets\image-20250619103257175.png)

5. 交换机绑定队列

![image-20250619104043167](.\assets\image-20250619104043167.png)

6. 队列绑定交换机

![image-20250619104234466](.\assets\image-20250619104234466.png)



> 添加配置文件: `config/autoload/amqp.php`

```php
<?php

return [
    'enable' => true,
    'default' => [
        'host' => '172.24.106.239',
        'port' => 5672,
        'user' => 'goer',
        'password' => 'goer',
        'vhost' => '/',
        'concurrent' => [
            'limit' => 1,
        ],
        'pool' => [
            'connections' => 1,
        ],
        'params' => [
            'insist' => false,
            'login_method' => 'AMQPLAIN',
            'login_response' => null,
            'locale' => 'en_US',
            'connection_timeout' => 3.0,
            // 尽量保持是 heartbeat 数值的两倍
            'read_write_timeout' => 6.0,
            'context' => null,
            'keepalive' => false,
            // 尽量保证每个消息的消费时间小于心跳时间
            'heartbeat' => 3,
            'close_on_destruct' => false,
        ],
    ],
];

```

> 修改ip和用户：rabbitMQ管理员用户不能消费的。



1. 创建一个生产者

```shell
php bin/hyperf.php gen:amqp-producer DemoProducer
```

```php
<?php

declare(strict_types=1);

namespace App\Amqp\Producer;

use Hyperf\Amqp\Annotation\Producer;
use Hyperf\Amqp\Message\ProducerMessage;

// exchange 交换机  routingKey 队列
#[Producer(exchange: 'hyperf', routingKey: 'test')]
class DemoProducer extends ProducerMessage
{
    public function __construct($data)
    {
        $this->payload = $data;
    }
}

```





2. 创建命令行

```shell
php bin/hyperf.php gen:command FooCommand
```

```php
<?php

declare(strict_types=1);

namespace App\Command;

use App\Amqp\Producer\DemoProducer;
use Hyperf\Amqp\Producer;
use Hyperf\Command\Command as HyperfCommand;
use Hyperf\Command\Annotation\Command;
use Hyperf\Context\ApplicationContext;
use Psr\Container\ContainerInterface;
use function Hyperf\Coroutine\co;

#[Command]
class FooCommand extends HyperfCommand
{
    public function __construct(protected ContainerInterface $container)
    {
        parent::__construct('demo:command');
    }

    public function configure()
    {
        parent::configure();
        $this->setDescription('Hyperf Demo Command');
    }

    public function handle()
    {

        $wg = new \Hyperf\Coroutine\WaitGroup();
        $wg->add(1000);

        for ($i = 0; $i <= 1000; $i++) {
            co(function () use ($wg) {

                $message = new DemoProducer(["date" => 1111]);
                $producer = ApplicationContext::getContainer()->get(Producer::class);
                $result = $producer->produce($message);

                $wg->done();
            });
        }


        $wg->wait();
    }
}

```

> 执行命令

```shell
bin/hyperf.php demo:command
```

> mq后台查看，已经生成了很多消息了
>
> 获取生产信息

![image-20250619105711006](.\assets\image-20250619105711006.png)



3. 消费者

> 项目启动，自动消费。

```shell
php bin/hyperf.php gen:amqp-consumer DemoConsumer
```

```php
<?php

declare(strict_types=1);

namespace App\Amqp\Consumer;

use Hyperf\Amqp\Result;
use Hyperf\Amqp\Annotation\Consumer;
use Hyperf\Amqp\Message\ConsumerMessage;
use PhpAmqpLib\Message\AMQPMessage;

#[Consumer(exchange: 'hyperf', routingKey: 'test', queue: 'test', name: "DemoConsumer", nums: 1)]
class DemoConsumer extends ConsumerMessage
{
    public function consumeMessage($data, AMQPMessage $message): Result
    {
        // 消费数据的逻辑
        var_dump($data);

        return Result::ACK;
    }
}

```

> 启动项目：十几秒就消费完了。
>
> 牛皮牛皮。

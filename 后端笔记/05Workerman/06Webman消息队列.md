<center>Webman消息队列</center>







[toc]







## Webman消息队列

> 消息队列(Message Queue)是一种异步通信的中间件技术，主要用于：

```shell
解耦系统组件
削峰填谷
异步处理
任务调度
日志处理
```





### 1. Redis队列

>  基于Redis的消息队列，支持消息延迟处理。

```shell
# 安装
composer require webman/redis-queue
```

redis配置文件自动生成在 `{主项目}/config/plugin/webman/redis-queue/redis.php`，内容类似如下：

```php
<?php
return [
    'default' => [
        'host' => 'redis://127.0.0.1:6379',
        'options' => [
            'auth' => '',         // 密码，可选参数
            'db' => 0,            // 数据库
            'max_attempts'  => 5, // 消费失败后，重试次数
            'retry_seconds' => 5, // 重试间隔，单位秒
        ]
    ],
];
```

> 创建生产者：
>
> `app/queue/redis/OrderProducer.php`

```php
<?php

namespace app\queue\redis;

use Webman\RedisQueue\Redis;

class OrderProducer
{
    /**
     * 发送订单消息到队列
     * @param array $data 订单数据
     * @return void
     */
    public static function send(array $data)
    {
        // 队列名
        $queue = 'order_queue';
        // 数据，可以直接传数组，无需序列化
        $data = ['order_id' => $data['order_id'], 'user_id' => $data['user_id'], 'amount' => $data['amount']];
        // 投递消息
        Redis::send($queue, $data);
        // 投递延迟消息，消息会在60秒后处理
        // Redis::send($queue, $data, 60);
    }
}
```

> 创建消费者
>
> 执行命令`php webman redis-queue:consumer order-consumer`则会生成文件`{主项目}/app/queue/redis/OrderConsumer.php`

```php
<?php 

namespace app\queue\redis;

use Webman\RedisQueue\Consumer;

class OrderConsumer implements Consumer
{

    // 要消费的队列名
    public $queue = 'order_queue';

    // 连接名，对应 plugin/webman/redis-queue/redis.php 里的连接`
    public $connection = 'default';

    /**
     * 消费队列消息
     * @param array $data
     * @return void
     */
    public function consume($data)
    {
        // 处理订单逻辑
        try {
            // 获取订单信息
            $orderId = $data['order_id'];
            $userId = $data['user_id'];
            $amount = $data['amount'];
            
            // 处理订单业务逻辑
            // ...
            
            // 记录日志
            \support\Log::info("Order processed successfully", [
                'order_id' => $orderId,
                'user_id' => $userId
            ]);
            
        } catch (\Exception $e) {
            // 异常处理
            \support\Log::error("Order process failed", [
                'error' => $e->getMessage(),
                'data' => $data
            ]);
            
            // 重要消息可以选择重新入队
            // Redis::send('order_queue', $data, 60);
        }
    }
}
```

> 配置消费进程： 
>
> 消费进程配置文件在 `{主项目}/config/plugin/webman/redis-queue/process.php`。

```php
<?php
return [
    'consumer'  => [
        'handler'     => Webman\RedisQueue\Process\Consumer::class,
        'count'       => 8, // 可以设置多进程同时消费
        'order_queue' => [
            'handler' => app\queue\redis\OrderConsumer::class,
            'count'   => 2, // 消费进程数
        ],
        'constructor' => [
            // 消费者类目录
            'consumer_dir' => app_path() . '/queue/redis'
        ]
    ]
];
```

> 使用：

```php
<?php
namespace app\controller;

use app\queue\producer\OrderProducer;

class OrderController
{
    public function create()
    {
        // 订单数据
        $orderData = [
            'order_id' => uniqid(),
            'user_id' => 1001,
            'amount' => 99.99
        ];
        
        // 发送到队列
        OrderProducer::send($orderData);
        
        return json(['code' => 0, 'msg' => 'Order created successfully']);
    }
}
```








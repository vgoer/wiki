<center>WebSocket</center>





[toc]







## WebSocket

> Hyperf 提供了对 WebSocket Server 的封装，可基于 [hyperf/websocket-server](https://github.com/hyperf/websocket-server) 组件快速搭建一个 WebSocket 应用。







### 1. 安装

```shell
composer require hyperf/websocket-server
```





### 2. 配置server

>  修改 `config/autoload/server.php`，增加以下配置。

```php
<?php

return [
    'servers' => [
        [
            'name' => 'ws',
            'type' => Server::SERVER_WEBSOCKET,
            'host' => '0.0.0.0',
            'port' => 9502,
            'sock_type' => SWOOLE_SOCK_TCP,
            'callbacks' => [
                Event::ON_HAND_SHAKE => [Hyperf\WebSocketServer\Server::class, 'onHandShake'],
                Event::ON_MESSAGE => [Hyperf\WebSocketServer\Server::class, 'onMessage'],
                Event::ON_CLOSE => [Hyperf\WebSocketServer\Server::class, 'onClose'],
            ],
        ],
    ],
];
```





### 2. 配置路由

> 目前暂时只支持配置文件的模式配置路由，后续会提供注解模式。

在 `config/routes.php` 文件内增加对应 `ws` 的 Server 的路由配置，这里的 `ws` 值取决于您在 `config/autoload/server.php` 内配置的 WebSocket Server 的 `name` 值。

```php
<?php

Router::addServer('ws', function () {
    Router::get('/', 'App\Controller\WebSocketController');
});
```







### 3. 配置中间件

在 `config/autoload/middlewares.php` 文件内增加对应 `ws` 的 Server 的全局中间件配置，这里的 `ws` 值取决于您在 `config/autoload/server.php` 内配置的 WebSocket Server 的 `name` 值。

```php
<?php

return [
    'ws' => [
        yourMiddleware::class
    ]
];
```







### 4. 创建控制器

```php
<?php
declare(strict_types=1);

namespace App\Controller;

use Hyperf\Contract\OnCloseInterface;
use Hyperf\Contract\OnMessageInterface;
use Hyperf\Contract\OnOpenInterface;
use Hyperf\Engine\WebSocket\Frame;
use Hyperf\Engine\WebSocket\Response;
use Hyperf\WebSocketServer\Constant\Opcode;
use Swoole\Server;
use Swoole\WebSocket\Server as WebSocketServer;

class WebSocketController implements OnMessageInterface, OnOpenInterface, OnCloseInterface
{
    public function onMessage($server, $frame): void
    {
        $response = (new Response($server))->init($frame);
        if($frame->opcode == Opcode::PING) {
            // 如果使用协程 Server，在判断是 PING 帧后，需要手动处理，返回 PONG 帧。
            // 异步风格 Server，可以直接通过 Swoole 配置处理，详情请见 https://wiki.swoole.com/#/websocket_server?id=open_websocket_ping_frame
            $response->push(new Frame(opcode: Opcode::PONG));
            return;
        }
        $response->push(new Frame(payloadData: 'Recv: ' . $frame->data));
    }

    public function onClose($server, int $fd, int $reactorId): void
    {
        var_dump('closed');
    }

    public function onOpen($server, $request): void
    {
        $response = (new Response($server))->init($request);
        $response->push(new Frame(payloadData: 'Opened'));
    }
}

```

> 看这个控制器：

```php
<?php
declare(strict_types=1);

namespace App\Controller;

use Hyperf\Contract\OnCloseInterface;
use Hyperf\Contract\OnMessageInterface;
use Hyperf\Contract\OnOpenInterface;
use Hyperf\Engine\WebSocket\Frame;
use Hyperf\Engine\WebSocket\Response;
use Hyperf\WebSocketServer\Constant\Opcode;
use Swoole\Server;
use Swoole\WebSocket\Server as WebSocketServer;

class WebSocketController implements OnMessageInterface, OnOpenInterface, OnCloseInterface
{
    public function onMessage($server, $frame): void
    {
        $response = (new Response($server))->init($frame);
        
        // 处理 PING 帧
        if($frame->opcode == Opcode::PING) {
            $response->push(new Frame(opcode: Opcode::PONG));
            return;
        }

        // 获取消息内容
        $message = $frame->data;
        
        // 可以解析 JSON 消息
        $data = json_decode($message, true);
        
        // 根据消息类型处理不同的业务逻辑
        if (is_array($data) && isset($data['type'])) {
            switch ($data['type']) {
                case 'chat':
                    // 处理聊天消息
                    $response->push(new Frame(payloadData: json_encode([
                        'type' => 'chat',
                        'message' => '收到聊天消息：' . $data['content']
                    ])));
                    break;
                    
                case 'heartbeat':
                    // 处理心跳消息
                    $response->push(new Frame(payloadData: json_encode([
                        'type' => 'heartbeat',
                        'status' => 'ok'
                    ])));
                    break;
                    
                default:
                    // 处理未知类型的消息
                    $response->push(new Frame(payloadData: json_encode([
                        'type' => 'error',
                        'message' => '未知的消息类型'
                    ])));
            }
        } else {
            // 处理普通文本消息
            $response->push(new Frame(payloadData: json_encode([
                'type' => 'message',
                'content' => '收到消息：' . $message
            ])));
        }
    }

    public function onOpen($server, $request): void
    {
        $response = (new Response($server))->init($request);
        
        // 1. 请求对象
        // var_dump($request);

        // 可以在这里进行连接验证，比如检查 token
        $token = $request->get['token'] ?? '';
        
        // 发送欢迎消息
        $response->push(new Frame(payloadData: json_encode([
            'type' => 'welcome',
            'message' => '连接成功！',
            'fd' => $request->fd
        ])));
    }

    public function onClose($server, int $fd, int $reactorId): void
    {
        // 可以在这里处理连接关闭后的清理工作
        // 比如从在线用户列表中移除该用户
        var_dump("客户端 {$fd} 已断开连接");
    }
}

```

> 前端

```js
// 获取或创建一个 WebSocket 连接
const websocket = new WebSocket('ws://127.0.0.1:9502');

// 连接成功
websocket.onopen = function(event) {
    console.log('WebSocket 连接成功:', event);
    
    // 发送普通文本消息
    websocket.send('Hello from frontend!');
    
    // 发送 JSON 格式的消息
    websocket.send(JSON.stringify({
        type: 'chat',
        content: '这是一条聊天消息'
    }));
    
    // 发送心跳消息
    setInterval(() => {
        websocket.send(JSON.stringify({
            type: 'heartbeat',
            timestamp: Date.now()
        }));
    }, 30000); // 每30秒发送一次心跳
};

// 接收消息
websocket.onmessage = function(event) {
    const data = JSON.parse(event.data);
    console.log('收到服务器消息:', data);
    
    // 根据消息类型处理不同的响应
    switch(data.type) {
        case 'welcome':
            console.log('欢迎消息:', data.message);
            break;
        case 'chat':
            console.log('聊天消息:', data.message);
            break;
        case 'heartbeat':
            console.log('心跳响应:', data.status);
            break;
        case 'error':
            console.error('错误消息:', data.message);
            break;
        default:
            console.log('其他消息:', data);
    }
};

// 连接关闭
websocket.onclose = function(event) {
    console.log('WebSocket 连接关闭:', event);
};

// 连接错误
websocket.onerror = function(error) {
    console.error('WebSocket 错误:', error);
};

// 示例：在某个事件（如点击按钮）中发送消息
// const sendButton = document.getElementById('send-button');
// sendButton.addEventListener('click', () => {
//     const messageInput = document.getElementById('message-input');
//     sendMessage(messageInput.value);
//     messageInput.value = ''; // 清空输入框
// });

```


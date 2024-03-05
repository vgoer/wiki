<center>websocket服务器</center>



[toc]







## websocket服务器

> 对于 Web 应用来说，最主流的当然就是我们之前学习过的 Http、TCP、UDP 这一类的应用。但是，最近这些年，特别是 HTML5 成为主流之后，WebSocket 应用日益丰富起来。要知道，之前我们在做后台时，如果要做消息通知之类的应用，全都是使用 JQuery 来进行 Ajax 轮询的。对于后台来说，这么做问题不大，但是如果你是要在前端页面做类似的功能，特别是一些客服功能的话，那可就费劲了。







### 1. 服务端

> WebSocket 服务也是继承自 Server 对象的

```php
//创建WebSocket Server对象，监听0.0.0.0:9501端口
$ws = new Swoole\WebSocket\Server('0.0.0.0', 9501);

//监听WebSocket连接打开事件
$ws->on('Open', function ($ws, $request) {
    while(1){
        $time = date("Y-m-d H:i:s");
        $ws->push($request->fd, "hello, welcome {$time}\n");
        
        // 让这个协程睡 10秒
        Swoole\Coroutine::sleep(10);
    }
});

//监听WebSocket消息事件
$ws->on('Message', function ($ws, $frame) {
    echo "Message: {$frame->data}\n";
    $ws->push($frame->fd, "server: {$frame->data}");
});

//监听WebSocket连接关闭事件
$ws->on('Close', function ($ws, $fd) {
    echo "client-{$fd} is closed\n";
});


$ws->start();
```

> 客户端：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>

<body>

<input type="text" id="txt"/><br/>
<button onclick="send()">发送</button>

<p id="response">

</p>

<script type="text/javascript">
      // ip 换位服务器开启的地址
      var wsServer = 'ws://172.24.106.239:9501';
      var websocket = new WebSocket(wsServer);
      websocket.onopen = function (evt) {
        console.log("Connected to WebSocket server.");
      };

      websocket.onclose = function (evt) {
        console.log("Disconnected");
      };

      websocket.onmessage = function (evt) {
        console.log('Retrieved data from server: ' + evt.data);
        document.getElementById("response").innerHTML = document.getElementById("response").innerHTML + 'Retrieved data from server: ' + evt.data + '<br/>';
      };

      websocket.onerror = function (evt, e) {
        console.log('Error occured: ' + evt.data);
      };


      function send(){
          websocket.send(document.getElementById('txt').value);
      }
</script>
</body>
</html>
```

> wc： 牛皮牛皮。
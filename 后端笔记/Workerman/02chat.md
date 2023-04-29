---
title: 02.chat
description: chat
published: 1
date: 2023-04-29T17:48:18.364Z
tags: workerman
editor: markdown
dateCreated: 2023-04-28T11:29:35.378Z
---

<center>chat</center>



[toc]







## chat

> 简单聊天功能

```php
# 服务端
<?php
use Workerman\Worker;
require_once __DIR__ . '/vendor/autoload.php';
$ws = new Worker('websocket://0.0.0.0:9993');

// 启动一个进程
$ws->count = 1;

$uuid = 0;

// 客户端连接上来时
$ws->onConnect = function($con){

    global $uuid;
    $con->uid = ++$uuid;
};

// 客户端发送信息来时
$ws->onMessage = function($con,$data){

    global $ws;
    foreach($ws -> connections as $conns){
        $conns->send("User_{$con->uid} said: $data");
    }
};

// 客户端断开后
$ws->onClose = function($con){

    global $ws;
    foreach($ws->connections as $conns){
        $conns->send("User_{$con->uid} logout");
    }
};
Worker::runAll();
```

> `php server.php start` 运行

```vue
# 客户端
<div id="app">
    <h2>{{message}}</h2>
    <hr>
    <ul>
        <li v-for="msg,index in resMessage" :key="index">
            {{msg}}
        </li>
    </ul>
    <hr>
    <form action="" @submit.prevent="sendSubmit">
        <input type="text" v-model="send">
        <input type="submit" value="send">
    </form>
</div>

<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
<script>
    const ws = new WebSocket('ws://127.0.0.1:9993');

    const {createApp} = Vue;

    createApp({
        data(){
            return{
                message: '简单聊天通讯',
                resMessage:['aa','bb'],
                send:'',
            }
        },
        mounted(){
            ws.onmessage = function(e){
                this.resMessage.push(e.data)
            }.bind(this)
        },
        methods:{
            sendSubmit:function(){
                console.log(this.send)
                ws.send(this.send)
                this.send = ''
            }

        }
    }).mount('#app')
</script>   
```


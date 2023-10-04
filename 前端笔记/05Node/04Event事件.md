---
title: 04.Event事件
description: Event事件
published: 1
date: 2023-06-09T10:13:32.360Z
tags: node
editor: markdown
dateCreated: 2023-04-22T18:28:31.383Z
---

<center>事件</center>

[toc]



## 事件

>nodeJs:
>
>1. 单进程，单线程，事件和回调支持并发，所以性能非常高
>
>2. API都是异步的
>3. 事件机制都是用设计模式中观察者模式实现。



> 机制：

```
Node.js的事件循环是靠一个单线程不断地查询队列中是否有事件，
当读取到事件时，将调用与这个事件关联的回调函数.
```



**事件生产者**：Node.js通过EventEmitter模块发送事件，发送的事件会被放到事件队列中。

**事件队列**：事件队列（Event Queue）是一个FIFO模型，一端用于接收推入的事件，另外一端拉出要处理的事件。

**事件循环**：事件循环（Event Loop）是Node.js事件机制的关键点，它一个单线程运行的任务，会不断轮询事件队列，并将轮询到的事件放到线程池中进行处理。

**线程池**：线程池（Thread Pool）是真正执行事件和任务处理的位置，比较耗时的操作如：网络I/O、文件操作I/O及其它会引起阻塞的操作都会在这里处理。处理完成后，会调用事件对应的回调函数。

![node_event_loop.png](https://cdn.jsdelivr.net/gh/dancefunk/quickask@1.0/dist/assets/img/node_event_loop.dd0f6c99.png)

### EventEmitter

```html
events 模块只提供了一个对象： events.EventEmitter。
EventEmitter 的核心就是事件触发与事件监听器功能的封装。
```

### 事件常用函数

```js
emitter.on(event, listener)		注册一个监听器
emitter.emit(event, [arg1], [arg2], [...])   执行一个监听器
emitter.once(event, listener)   注册一个单次监听器
emitter.removeListener(event, listener) 移除指定事件的某个监听器
emitter.removeAllListeners([event])   移除所有事件的所有监听器
emitter.listeners(event)  返回指定事件的监听器数组。
emitter.setMaxListeners(n)  用于提高监听器的默认限制的数量
EventEmitter.listenerCount(emitter, event) 返回指定事件的监听器数量。
...
```

> 注册监听器

```js
/*
调用模块，获取events.EventEmitter对象
*/
var Even = require('events').EventEmitter;
var eve = new Even;
/*
    EventEmitter.on(event, listener) 为事件注册一个监听
    参数1：event  字符串，事件名
    参数2：回调函数
*/
eve.on('some',function(name,age){
	console.log(`姓名：${name},年龄:${age}`);
})
/*
    EventEmitter.emit(event, [arg1], [arg2], [...])   触发指定事件
    参数1：event  字符串，事件名
    参数2：可选参数，按顺序传入回调函数的参数
    返回值：该事件是否有监听
*/
eve.emit('some','blue',20);  //执行监听

var beve = eve.emit('some','red',10); // 打印true
```



> 删除监听器

```js
let Even = require('events').EventEmitter;
let eve = new Even();

//普通事件
function name(name){
    console.log(`姓名:${name}`);
}
function age(age){
    console.log(`年龄:${age}`);
}
// 注册监听
eve.on('dome',name);
eve.on('dome',age);

eve.emit('dome','abc'); //触发事件

eve.removeListener('dome',name); //删除 'dome',name 事件
eve.removeAllListeners('dome');   //删除所有dome事件

eve.emit('dome','ABC');  
```



> 查看监听器

```js
let Even = require('events').EventEmitter;
let eve = new Even();

//普通事件
function name(name){
    console.log(`姓名:${name}`);
}
function age(age){
    console.log(`年龄:${age}`);
}
// 注册监听
eve.on('dome',name);
eve.on('dome',age);
/*
    EventEmitter.listeners(event)   //返回指定事件的监听数组
    参数1：event  字符串，事件名    
*/

var evelist = eve.listeners('dome');  //监听器数组

console.log(evelist);
console.log(evelist.length);
```

> 设置最大监听数量

```js
/*
    EventEmitter.setMaxListeners (n)   给EventEmitter设置最大监听
    参数1： n 数字类型，最大监听数
    
    超过10个监听时，不设置EventEmitter的最大监听数会提示：
    (node) warning: possible EventEmitter memory leak detected. 11 listeners added.
     Use emitter.setMaxListeners() to increase limit.
    设计者认为侦听器太多，可能导致内存泄漏，所以存在这样一个警告
*/
eve.setMaxListeners(30);  //最大监听数
for(let i=20; i>0;i--){
	eve.on('dome',(a)=>{
		console.log(`第${i}个${a};`);
    });
}
eve.emit('dome','blue');  //没有设置最大监听报错
```

> 监听器连接事件

```js
var events = require('events');
var eve = new events.EventEmitter();

var lis1 = function lis1(){
	console.log('监听器1');
}
eve.addListener('dome',lis1);//监听器绑定 
//和一样
//eve.on('dome',lis1);
evelists = require('events').EventEmitter.listenerCount(eve,'dome');
console.log(evelists);  //1个
```


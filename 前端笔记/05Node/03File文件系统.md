---
title: 03.File文件系统
description: File文件系统
published: 1
date: 2023-06-09T10:13:30.796Z
tags: node
editor: markdown
dateCreated: 2023-04-22T18:26:18.486Z
---

<center>文件系统</center>

[toc]

## file

> 标准的文件操作API

```js
const fs = require("fs");  //导入模块
```



### 读取文件

> 异步和同步 ,一般用**异步**

```js
const fs = require("fs");

//同步是代码一行一行执行
var data = fs.readFileSync('aa.txt');
console.log("同步"+data.toString());

//异步
//两个参数  文件  返回函数
fs.readFile('a.txt',function(err,data){
	if(err){
		return console.error(err)
    }
    console.log('同步'+data.toSting());
	
});
```



### 打开文件

> fs.open(path, flags[, mode], callback)

```
- path - 文件的路径
- flags - 文件打开的行为
- mode - 设置文件模式(权限)，文件创建默认权限为 0666(可读，可写)
- callback - 回调函数，带有两个参数如：callback(err, fd)
```

| Flag | 描述                                                     |
| :--- | :------------------------------------------------------- |
| r    | 以读取模式打开文件。如果文件不存在抛出异常。             |
| r+   | 以读写模式打开文件。如果文件不存在抛出异常。             |
| rs   | 以同步的方式读取文件。                                   |
| rs+  | 以同步的方式读取和写入文件。                             |
| w    | 以写入模式打开文件，如果文件不存在则创建。               |
| wx   | 类似 'w'，但是如果文件路径不存在，则文件写入失败。       |
| w+   | 以读写模式打开文件，如果文件不存在则创建。               |
| wx+  | 类似 'w+'， 但是如果文件路径不存在，则文件读写失败。     |
| a    | 以追加模式打开文件，如果文件不存在则创建。               |
| ax   | 类似 'a'， 但是如果文件路径不存在，则文件追加失败。      |
| a+   | 以读取追加模式打开文件，如果文件不存在则创建。           |
| ax+  | 类似 'a+'， 但是如果文件路径不存在，则文件读取追加失败。 |

```js
// 打开文件
fs.open('a.txt','a+',(err,fd)=>{
	if(err){
		return console.error(err);
    }
    console.log('文件创建成功');
});
```



### 获取文件信息

> fs.stat(path,callback)

```
path - 文件路径
callback - 回调函数，带有两个参数如：(err, stats), stats 是 fs.Stats 对象
```

```js
fs.stat('a.txt',function(err,stats){
	if(err){
		return console.error(err);
    }
    console.log(stats); //打印文件信息
    console.log(stats.isDirectory());//是否是文件
});
```

| 方法                      | 描述                                                         |
| :------------------------ | :----------------------------------------------------------- |
| stats.isFile()            | 如果是文件返回 true，否则返回 false。                        |
| stats.isDirectory()       | 如果是目录返回 true，否则返回 false。                        |
| stats.isBlockDevice()     | 如果是块设备返回 true，否则返回 false。                      |
| stats.isCharacterDevice() | 如果是字符设备返回 true，否则返回 false。                    |
| stats.isSymbolicLink()    | 如果是软链接返回 true，否则返回 false。                      |
| stats.isFIFO()            | 如果是FIFO，返回true，否则返回 false。FIFO是UNIX中的一种特殊类型的命令管道。 |
| stats.isSocket()          | 如果是 Socket 返回 true，否则返回 false。                    |



### 写入文件

> fs.writeFile(path, data[, options], callback)  `该方法写入的内容会覆盖旧的文件内容。`

```
path - 文件路径

data - 要写入文件的数据，可以是 String(字符串) 或 Buffer(流) 对象
options - 该参数是一个对象，包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'

callback - 回调函数，回调函数只包含错误信息参数(err)，在写入失败时返回
```

```js
fs.wirteFile('a.txt','写入内容',fucntion(err){
	if(err){
		return console.error(err);
	}
	console.log('写入成功');
});
```



### 读取文件

> - fs.read(fd, buffer, offset, length, position, callback)

```
fd - 通过 fs.open() 方法返回的文件描述符。
buffer - 数据写入的缓冲区。
offset - 缓冲区写入的写入偏移量
length - 要从文件中读取的字节数
position - 文件读取的起始位置，如果 position 的值为 null，则会从当前文件指针的位置读取
callback - 回调函数
```

```js
// 读取文件
var buf = new Buffer(1000);
fs.open('bb.tt','r+',function(err,fd){
    if(err){
        return console.error(err);
    }
    fs.read(fd,buf,0,buf.length,0,function(err,byts){
        if(err){
            return console.error(err);
        }
        console.log(byts+"字节");

        if(byts > 0){
            console.log(buf.slice(0,byts).toString());// 读取文件内容
        }
    });
});
```

### 关闭文件

> fs.close(fd, callback)

```
fd - 通过 fs.open() 方法返回的文件描述符。
callback - 回调函数，没有参数。
```



### 截取文件

> fs.ftruncate(fd, len, callback)

```
fd - 通过 fs.open() 方法返回的文件描述符
len - 文件内容截取的长度
callback - 回调函数，没有参数
```

```js
//截取
fs.ftruncate(fd,10,funtion(err){
	//截取10个字符
})
```


---
title: 14.Brup
description: brup web的瑞士军刀
published: 1
date: 2023-04-21T05:02:46.885Z
tags: web safe, brup
editor: markdown
dateCreated: 2023-04-19T17:34:12.188Z
---

<center>Burp Suite <center>

[toc]

## Burp

> 攻击web 应用程序的集成平台。 抓包、截包、改包

burp:[官网](https://portswigger.net/burp)



### Proxy

> 代理模块

1. Options 

```
* 设置代理监听的ip和端口  127.0.0.1：任意
* 浏览器 高级  网络  设置一样的端口和ip
```

2. Intercept

```
* intercept is on/off 截断的开关
* Forward:将请求包发送
* Drop:丢掉请求包
* Action对请求包的一些操作
```

3. Http history

```
* 请求历史和site map一样
```



### Target

> 目标模块

1. Site map

```
* 目标历史 请求内容
* 筛选请求，标记请求，导出历史
```

2. Scope

```
* 过滤的
```

3. lssue definitions

```
* 重要的，主流的漏洞
```



### Repeater

> 改包，发包 模块

```
* 重复的发包改包，
* 然后Send 发送
```



> 抓https的包，设置 `SSL Proxy` 代理
>
> 首先抓http的包下载burp证书，`http://burp`
>
> 导入证书，就可以了
>
> ` HTTP Proxy ` :只能抓http包
>
> 设置 `SSL Proxy` 代理 ：可以抓https的包



### Intruder  **

> 暴力破解 **重要**

1. Attack Target

```
攻击目标
* host 主机
* port 端口
* use http/https等
```

2. Positions

```
// 设置参数，
设置变量， attack type:四种方式是遍历变量的
```

3. Payloads

```
// 这三个都很重要
* payload sets  //设置 每个变量的 payload type
* optiond list  //字典和文件 
* processing  //编码这些的
```

**Start attack** -----> 开始攻击，`看length长短`，不一样的可能就是对的





### Decoder

> 编码解码模块

```
decode as //编码
Encode as // 解码
Hash //散列
Smart decode //智能解码
```



### Comparer

> 对比模块

```
* 对比文本发到comparer
* words 对比
```





### Extender

> 扩展模块

```
* Exte sions 安装插件等
* Bapp Store 插件商店
```



### User options

> 用户设置




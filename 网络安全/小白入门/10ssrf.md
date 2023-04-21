---
title: 10.ssrf
description: 服务端请求伪造
published: 1
date: 2023-04-21T05:11:55.612Z
tags: web safe
editor: markdown
dateCreated: 2023-04-19T17:29:36.043Z
---

<center>ssrf</center>

[toc]

## ssrf

> 服务端请求伪造（Server-Side Request Forgery）
>
> 攻击者构建攻击链传给服务器，服务器执行并请求造成的安全漏洞
>
> 外网探测内网



**robots.txt**

> 爬虫：告诉正规爬虫不要爬这些目录
>
> > 这些目录就是应该考虑的隐私文件



> 构造

```php
// index.php

if($_GET['path']){
	$url = $_GET['path'];
}
// echo $url;
$cont = file_get_contents($url); //获取文件内容

echo $cont;
```

```php
// 任意文件 robots.txt
url?path=www.baidu.com/robots.txt
//这样这个服务器就可以查看其他服务器的文件
//因为规顶同一资源定位符 每个资源都有一个唯一的url
url?img=****/a.png;
```

> 可以查看内网服务，端口等


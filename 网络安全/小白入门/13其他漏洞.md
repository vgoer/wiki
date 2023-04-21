---
title: 13.其他漏洞
description: 其他漏洞
published: 1
date: 2023-04-19T17:33:02.455Z
tags: web safe
editor: markdown
dateCreated: 2023-04-19T17:32:57.174Z
---

<center>其他漏洞</center>

[toc]



## 漏洞篇



### 信息泄露

> 1. 敏感信息(它不想让你知道的)
>
> 2. 账号密码
> 3. 网站源码
> 4. 数据库信息 等等

```php
//读取服务器文件
/index.php?f=php://filter/convert.base64-encode/resource=index.php
/index.php?f=file://D://www/html/about.php
```

> 收集：`inurl:fieltype mdb ` 数据库文件



### 身份认证

> 利用你的身份

### 命令执行

> 在服务器端执行了命令



### 逻辑漏洞(越权)

> 写程序没有考虑周全，有逻辑漏洞



> 越权漏洞：权限不是超过自己



### 文件上传

> 文件上传到服务器

```
校验别人文件：
*前端校验
*后端校验

//文件后缀，大小，内容，自定义规则等等
```



### 路由器(ap)漏洞

> web：也是带有web服务
>
> linux: 路由器也是linux





### Fuzz模糊测试

> 带有暴力破解的意思
>
> 猜是否带有漏洞，收集字典




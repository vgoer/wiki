---
title: 11.xxe
description: xxe可扩展标记语言
published: 1
date: 2023-04-19T17:30:38.277Z
tags: web safe
editor: markdown
dateCreated: 2023-04-19T17:30:32.118Z
---

<center>xxe</center>

[toc]

### xxe 

> xml外部实体注入漏洞(XML External Entity Injection)
>
> > 发生  `解析xml输入时`没用**禁用外部实体的加载**
>
> 读取外部文件，内网探测扫描等等



#### xml

> 可扩展的标记语言：标签名很**随意**

```
<?xml vsersion='1.0' encoding='utf-8'>
<!DOCTYPE 家庭 SYSTEM 'a.dtd'// 引入a.dtd文件约束xml文件
<家庭>
    <成员>
        <名字>a</名字>
        <年龄>10</年龄>
        <爱好>打游戏</爱好>
    </成员>

    <成员>
        <名字>b</名字>
        <年龄>19</年龄>
        <爱好>打篮球</爱好>
    </成员>
</家庭>
```



#### dtd

> 文档类型定义（Document Type Definition）规定好的
>
> js - 弱类型 --  随意
>
> ts -- 强类型 -- 严格的定义

`xml`太随意了 用`dtd`来约束

```
// a.dtd  约束

```

文档：[dtd](https://blog.csdn.net/gavin_john/article/details/51532756)

 

> 防护：因为没用禁用实体加载

```
echo LIBXML_DOTTED_VERSON; //查看版本
//禁用实体加载
libxml_disable_entity_loader(trun); //禁用   false:应许
```








---
title: 01.search
description: search
published: 1
date: 2023-06-09T10:17:20.684Z
tags: google
editor: markdown
dateCreated: 2023-04-29T17:54:11.114Z
---

<center>搜索语法</center>

[toc]



## 简单的

> 搜索引擎是生活和工作中必备的工具 [blog](https://zhuanlan.zhihu.com/p/136076792) [zhihu](https://zhuanlan.zhihu.com/p/136076792)
>
> [博客园](https://www.cnblogs.com/langtianya/p/4513408.html)

#### 搜索语法：

利用好它我们能找到==出乎意料==的信息



> > 0. " 关键字 " or "关键字"

```shell
"taobao" or "admin"
```





> > 1. filetype (搜索文件的后缀名或扩展名)

```url
filetype:

eg: filetype:xls
# 返回xls结尾的excel文件的url地址
# value: xls doc zip pdf等等
```



> > 2. info （网站基本信息）

```url
info:

eg: info:www.baidu.com
#获取网站基本信息
#value: url
```



> > 3. inurl (搜索网站链接包含的关键字)

```url
inurl:

eg: url:admin
# 搜索网站的后台管理员admin登录地址
# 这个很厉害哦！
```



> > 4. index of (对搜索引擎结果进行二次检索)

```url
index of
		直接进入网站首页下的文件和文件夹
		
eg: index 0f /admin
# 搜索网站的后台登录地址
```



> > 5. intext (网页内容信息)

```
intext:

eg: intext:关键字
# 搜索网页的正文内容，类似我们直接搜
```



> > 6. intitle (网页title信息)

```url
intitle:
		了解前端知识的同学们应该很清楚，就是网页html中那个<title>标签内
		
eg: intitle:关键字
# 搜索网页标题中包含关键字的
```



> > 7. cache (搜索引擎缓存信息)

```url
cache:
		搜索引擎关于某项关键字的缓存信息，emmm有可能会发现一些很有趣的东西

eg: cache:关键字
#浏览器缓存的信息
```



| 语法                   | 简介                                                 | 实例                          | 作用                                                         |
| ---------------------- | ---------------------------------------------------- | ----------------------------- | ------------------------------------------------------------ |
| `"关键词"`             | 精确匹配关键词短语                                   | `"人工智能"`                  | 仅返回包含精确短语“人工智能”的结果                           |
| `关键词1 OR 关键词2`   | 返回包含关键词1或关键词2（或两者）的结果             | `Python OR Java`              | 返回包含“Python”或“Java”（或两者）的结果                     |
| `关键词1 AND 关键词2`  | 返回同时包含关键词1和关键词2的结果 (隐式，可省略AND) | `机器学习 深度学习`           | 返回同时包含“机器学习”和“深度学习”的结果                     |
| `*`                    | 通配符，匹配任何单词                                 | `学习*方法`                   | 匹配包含“学习”后跟任何单词再跟“方法”的结果，例如“学习编程方法”、“学习英语方法” |
| `site:网站域名 关键词` | 仅在指定网站内搜索                                   | `site:wikipedia.org 量子力学` | 仅在维基百科搜索“量子力学”相关内                             |
| `filetype:文件类型`    | 指定搜索结果的文件类型                               | `filetype:pdf 教程`           | 仅返回PDF格式的“教程”文件                                    |
| `intitle:关键词`       | 搜索标题中包含指定关键词的网页                       | `intitle:Python 教程`         | 返回标题中包含“Python 教程”的网页                            |
| `inurl:关键词`         | 搜索URL中包含指定关键词的网页                        | `inurl:python 教程`           | 返回URL中包含“python 教程”的网页                             |
| `intext:关键词`        | 搜索网页正文中包含指定关键词的网页                   | `intext:深度学习算法`         | 返回网页正文中包含“深度学习算法”的网页                       |
| `related:网站域名`     | 查找与指定网站相关的网站                             | `related:google.com`          | 查找与Google相关的网站                                       |
| `cache:网址`           | 查看网页的缓存版本                                   | `cache:www.baidu.com`         | 查看百度网站的缓存版本                                       |










---
title: 18.sqlmap
description: sqlmap注入神器
published: 1
date: 2023-04-21T05:12:20.260Z
tags: web safe, sqlmap
editor: markdown
dateCreated: 2023-04-19T17:45:04.766Z
---

<center>Sqlmap</center>



[toc]







## sqlmap

> 开源渗透工具，自动检测和利用sql注入漏洞 [github](https://github.com/sqlmapproject/sqlmap)



### 1. 使用

> 参数: [usage](https://github.com/sqlmapproject/sqlmap/wiki/Usage)

> target

```shell
sqlmap -h

# url
sqlmpa -u url


# 有提示的
sqlmap --wizard 

# -m 文件   * 所参数
www.target1.com/vuln1.php?q=foobar
www.target2.com/vuln2.asp?id=1
www.target3.com/vuln3/id/1*

# -r 请求测试
POST /vuln.php HTTP/1.1
Host: www.target.com
User-Agent: Mozilla/4.0
id=1

# -g 谷歌
sqlmap -g "inurl:\".php?id=1\""
```



> request

```shell
# http方法和data  --method=  --data 重要
# 提交post
sqlmap -u "url" --method=post --date="id=1"

# --param-del 分隔符
sqlmap -u "url" --data="query=foobar;id=\
1" --param-del=";" -f --banner --dbs --users

# --cookie= 设置cookie

# --user-agent=  请求头：浏览器信息 重要
sqlmap -u "url" --user-agent="浏览器信息"

# --random-agent 随机 浏览器信息 重要
sqlmap -g "inurl:\".php?id=1\"" --random-agent

# --referer 网站的来源 重要
--referer=
# --headers= http头
# --proxy= 代理
```



> injection 注入

```shell
# --level 3 等级越高测试越多

```



> techniques 注入技术

```shell
```



> enumeration 枚举

```shell
# --users 列出注入用户

# --tables 表  

# --current-db 当前数据库 

# --passwords 密码

# --schema 系统库 --dump 数据dump出来
```



> 常用命令

```shell
# 常用
sqlmap -u url  
sqlmap -u url --tamper randomcase.py # 混淆代码  /usr/share/sqlmap/tamper 其他代码
sqlmap -u url --level 4 

# 权限
sqlmap -u url --privileges 
sqlmap -u url --is-dba #个人权限


# 查看库 表
--dbs/--current-db # 数据库
# 查看 某个库的表
sqlmap -u url -D database --table
# 查看表字段
sqlmap -u url -D database -T table --columns
# 内容
sql -u url -D datables -T table -C name --dump

# 查看用户
--user
# 密码
--passwords
# 系统表
--schema
# shell  实际上传木马
--os-shell
```




























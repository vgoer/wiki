---
title: 03连接github
description: 
published: 1
date: 2023-06-17T01:21:36.170Z
tags: 
editor: markdown
dateCreated: 2023-06-17T01:21:34.812Z
---

<center>连接github</center>

[toc]


## 密钥连接github
> 不要使用不安全的用户名密码 使用ssh密钥



### git密钥连接github

> git

```shell
# 你的邮箱
ssh-keygen -t rsa -b 4096 -C "hi_goer@163.com"

# 查看公钥和私钥  id_rsa 私钥   id_rsa.pub 公钥
~/.ssh 

# 复制公钥到 github
cat ~/.ssh/id_rsa.pub

# 复制到
Settings”>“SSH and GPG keys”>“New SSH key”

# 测试
ssh -T git@github.com

# 提交
echo "# hyperf-redis-mysql" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin git@github.com:vgoer/hyperf-redis-mysql.git
git push -u origin master
```






---
title: 05.nginx
description: nginx
published: 1
date: 2023-05-29T04:11:08.499Z
tags: docker env
editor: markdown
dateCreated: 2023-04-22T05:19:45.705Z
---

<center>Nginx</center>





[toc]





## Nginx

> web服务器
>
> [hub](https://hub.docker.com/)



```shell
# 拉取最小镜像
docker pull nginx:stable-alpine

# 启动
docker run -d -p 8001:80 --name nginx nginx:stable-alpine
# 访问  ip:8001

# 查看配置目录 /etc/nginx  nginx.conf 配置文件  conf.d 默认目录配文件
docker exec -it nginx ls -al /etc/nginx

# 网站目录 /usr/share/nginx/html
docker exec -it nginx ls -al /usr/share/nginx/html
```


---
title: 06.python环境
description: python环境
published: 1
date: 2023-04-22T05:21:27.096Z
tags: docker env
editor: markdown
dateCreated: 2023-04-22T05:21:24.147Z
---

<center>pthon</center>



[toc]





## python

> docker搭建python环境



```shell
docker pull python:3.7.13-alpine

docker run -itd --name python python:3.7.13-alpine

# 进入容器
docker exec -it python sh
docker exec -it python python3 -v  # pip3 -v
```


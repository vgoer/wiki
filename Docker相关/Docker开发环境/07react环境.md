---
title: 07.react环境
description: react环境
published: 1
date: 2023-05-05T11:45:47.633Z
tags: docker env
editor: markdown
dateCreated: 2023-04-22T05:22:17.991Z
---

<center>React</center>



[toc]





## React

> docker 搭建环境React 环境



```shell
# 项目管理
mkdir app
nano Dockerfile
nano docker-compose.yml
```

```shell
FROM node:16.17-alpine
ADD ./app /app 
WORKDIR /app
```

```yaml
version: '3.8'

services:
  node: 
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ./app:/app
    command: sh -C "cd react-sample && npm start"
    ports:
      - "3000:3000"
    stdin_open: true #标准输入
```

> 启动

```shell
# 新镜像
docker-compose build 
# 创建app
docker run --rm docker-node sh -c "npm install -g create-react-app && create-react-app react-sample"
# 启动
docker-compose up -d
```




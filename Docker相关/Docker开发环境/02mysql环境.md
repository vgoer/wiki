---
title: 02.mysql环境
description: mysql环境
published: 1
date: 2023-04-29T17:42:24.771Z
tags: docker env
editor: markdown
dateCreated: 2023-04-22T05:05:15.527Z
---

<center>mysql</center>



[toc]





## mysql

> docker搭建mysql环境



```shell
# mysql目录
mkdir mysql
# 数据库初始化脚本
mkdir mysql/nitdb
# 数据库存储目录
mkdir mysql/datadir
# my.cnf 配置文件
nano mysql/my.cnf
```

```shell
[mysqld]
charecter-set-server=utf8
[mysql]
default-character-set=utf8
[client]
default-character-set=utf8
```

> `nano docker-compose.yaml`

```shell
version: '3'
 
services:
  mysql:
    image: mysql:5.7
    container_name: web_mysql
    environment:
      # 配置信息
      MYSQL_DATABASE: testsql
      MYSQL_ROOT_USER: root
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_USER: test
      MYSQL_PASSWORD: test123
      TZ: 'Asia/shanghai'
    ports:
      - "6033:3306"
    volumes:
      - ./initdb:/docker-entrypoint-initdb.d # 启动数据设定
      - ./datadir:/var/lib/mysql
      - ./my.cnf:/etc/mysql/conf.d/my.cnf
    restart: always
  phpmyadmin:
    image: phpmyadmin:latest
    container_name: web_myadmin
    links:
      - mysql
    ports:
      - 80:80
    environment:
      # 可以跨域登录
      # PMA_ARBITRARY: 1
      # 网络
      PMA_HOST: mysql
      # 自动提供用户名和密码
      # PMA_USER: test
      # PMA_PASSWORD: test123
    restart: always

```


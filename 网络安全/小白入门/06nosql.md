---
title: 06.nosql
description: nosql非关系型数据库
published: 1
date: 2023-06-09T10:16:56.728Z
tags: web safe, nosql
editor: markdown
dateCreated: 2023-04-19T17:23:19.108Z
---

<center>NoSql</center>

[toc]

## NoSql

> Nosql: 非关系型数据库。
>
> 用于超大规模数据存储。





### 1. 启动mongodb

> `docker-compose.yaml`

```yaml
services:
  mongodb:
    image: mongo:latest
    container_name: mongodb
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
    ports:
      - "27017:27017"
    volumes:
      - mongodb_data:/data/db

volumes:
  mongodb_data:
```

```shell
# 启动
docker-compose up -d

# 进入容器：
mongosh "mongodb://admin:password@localhost:27017"
```

> 方法二： arch

```shell
yay -S mongodb
# 或者
yay -S mongodb-bin
```







### 2. 使用

```shell
# 1. 查看数据库， 一般用前面的
show dbs;   /  show databases;

# 2. 使用 和 新建数据库
use 数据库;

# 3. 查看表
show collections;  / show tables;

# 4. 查看
db.数据表.find();
# 格式化
db.数据表.find().pretty();
#  name=admin
db.user.find({"name" : "user1"});

# 5. 插入数据 和 新建表
db.goods.insert({"name": "goer", "age": 18});


```








<center>nosql注入</center>

[toc]

## nosql

> `非关系型数据库`，  用于超大规模数据存储



### mogod

> 数据库：

> docker搭建

```shell
docker run --name mongodb -p 27017:27017 -d mongo
docker exec -it mongodb bash
mongo
```

```sql
// 安装好
// 开启服务
mongod --dbpath /路径  // 指定数据存放的位置

// 连接
mongo

//查看数据库
show databases;
show dbs;

//使用数据库
use database;//数据库

// 查看数据库里的表
show tables;
show collections; //多用这个

// 查看表中的数据
db.user.find(); //user表   json格式的
db.user.fint().pretty(); 格式好看些
// 不能用 select * from user;

// 查看表中一组数据  
db.user.find({"name":"user1"}); //查到name=user1这条数据
// 类似 select * from user where name=user1;
```

> 滴滴-- 发车了

```sql
// 创建数据库
use test; // 因为里面没有东西，所有看不见数据库

// 插入表
db.goods.insert({"name":"user","age":"20"}); 
//新建表，插入两个字段
```

[博客](https://blog.szfszf.top/article/27/)




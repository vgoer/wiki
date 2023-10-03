---
title: 37.go&mongoDb
description: go&mongoDb
published: 1
date: 2023-06-09T10:12:33.537Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T10:04:15.213Z
---

<center>MongoDb</center>



[toc]



## MongoDB

> MongoDB是一个基于分布式文件存储的数据库，由C++语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。



### 1.下载

> MongoDB:[mongo](https://www.mongodb.com/try/download/community)

```go 
// 进入到安装目录  如果系统不一样百度一下  第二个自定义安装
// 默认  C:\Program Files\MongoDB\Server\6.0\bin 或者下载压缩包
mongo.exe 启动了

1. 创建数据库
use gostudydb 

2. 创建集合
db.createCollection("sut")
```



### 2. 连接

> mongo[pkg](https://pkg.go.dev/go.mongodb.org/mongo-driver#section-readme)

```go
// 1. 安装
go get go.mongodb.org/mongo-driver/mongo
```

```go 
// 导入包 检测
package main

import (
	"context"
	"fmt"
	"log"

	"go.mongodb.org/mongo-driver/mongo"
	"go.mongodb.org/mongo-driver/mongo/options"
)

// 新建指针对象
var client *mongo.Client

// 连接函数
func initDB() {
	// 设置连接配置
	cliOptions := options.Client().ApplyURI("mongodb://localhost:27017")
	// 连接数据库
	client, err := mongo.Connect(context.TODO(), cliOptions)
	if err != nil {
		log.Fatal(err)
	}

	// 检查连接
	err2 := client.Ping(context.TODO(), nil)
	if err2 != nil {
		log.Fatal(err2)
	} else {
		fmt.Println("数据库连接成功")
	}
}
```



### 3. Bson

> mongo中json文档存储在名为BSON(二进制编码的json中)

```go
// 导入包 
	"go.mongodb.org/mongo-driver/mongo/bson"
```



### 4. 插入

> 插入数据

```go
```










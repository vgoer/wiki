---
title: 01.xorm简介
description: xorm简介
published: 1
date: 2023-04-22T04:49:21.614Z
tags: golang
editor: markdown
dateCreated: 2023-04-22T04:49:18.564Z
---

<center>xorm</center>



[toc]



## xorm

> 强大的go语言orm库，orm数据库表和语言对象映射





### 1. 安装

```go
go mod init xorm
go get xorm.io/xorm
go get github.com/go-sql-driver/mysql
```



### 2. 使用

```go
import (
	"fmt"

	_ "github.com/go-sql-driver/mysql"
	"xorm.io/xorm"
)

var engine *xorm.Engine
var err error
func main() {
	// 					连接数据库 mysql   用户名 密码  数据库 字符集
	engine, err = xorm.NewEngine("mysql", "root:root@/golang?charset=utf8")
	if err != nil {
		fmt.Printf("err: %v\n", err)
	} else {
		// 连接成功
		err2 := engine.Ping()
		if err2 != nil {
			fmt.Println("err2:%v\n", err2)
		} else {
			print("连接成功")
		}
	}
}
```

1. 创建表

```go
// 结构体同步为表
type goUser struct {
	Id      int64
	Name    string
	Age     string
	Passwd  string    `xorm:"varchar(200)"`
	Created time.Time `xorm: "created"`
	Update  time.Time `xorm: "updated"`
}

// 创建表
err3 := engine.Sync(new(goUser))
fmt.Println("err3:%v\n", err3) // <nil> 成功了
```



### 2.表结构定义

```go
1. 表名称
setMapper(names.GonicMapper{})  SnakeMapper{} 都是 命名加下划线   goUser -> go_user
setMapper(names.SameMapper{}) SameMapper 和结构体名称一样

2. 设置表前缀和后缀
tbMapper := names.NewPrefixMapper(names.SnakeMapper{}, "golang_")
engine.SetTableMapper(tbMapper)
```



### 3. 表结构操作

> 比较少用，一般数据库操作

```go
1. 数据库表信息 DBMetas() 

	t, _ := engine.DBMetas()
	for _, v := range t {
		// fmt.Printf("v:%v\n", v.Name)
		// 表信息
		table, _ := engine.TableInfo(v)
		fmt.Printf("table:%v\n", table)
	}
2. 同步数据库 常用的
   engine.Sync2(new(goUser), new(goUser1))
3. 导出数据库
   engine.DumpAllToFile("a.sql")
4. 导入
	engine.ImportFile("a.sql")
```
















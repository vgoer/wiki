---
title: 02.crud
description: crud
published: 1
date: 2023-04-22T04:51:00.043Z
tags: golang
editor: markdown
dateCreated: 2023-04-22T04:50:57.248Z
---

<center>crud</center>



[toc]





## crud

> 增删改查



### 1. 添加

```go
import (
	"fmt"
	"time"

	_ "github.com/go-sql-driver/mysql"
	"xorm.io/xorm"
)

// 结构体  `xorm:"updated"`
type goUsers struct {
	Id      int64
	Name    string
	Age     string
	Passwd  string    `xorm:"varchar(200)"`
	Created time.Time `xorm:"created"`
	Updated time.Time `xorm:"updated"`
}

type goTables struct {
	Id      int64
	Name    string
	Age     string
	Passwd  string    `xorm:"varchar(200)"`
	Created time.Time `xorm:"created"`
	Updated time.Time `xorm:"updated"`
}

var engine *xorm.Engine
var err error

func init() {
	// 					连接数据库 mysql   用户名 密码  数据库 字符集
	engine, err = xorm.NewEngine("mysql", "root:root@/go?charset=utf8")
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
func main() {
	user := new(goUsers)
	user.Age = "20"
	user.Name = "tom"
	user.Passwd = "tom123"

	table := goTables{
		Name:   "tb",
		Age:    "20",
		Passwd: "tb123",
	}

	//创建表
	engine.Sync2(new(goUsers), new(goTables))
	// 查看sql语句
	engine.ShowSQL(true)
	// 插入数据  Insertont 插入一条  new(goUsers) 已经是指针了
	engine.Insert(user, &table)
}

```



### 2. 查询

> 熟悉基本的sql语句 

```go
1. Alias(string)  别名 as
2. And(string,interface{})  并且 and
3. Where(string) 查询条件
4. Asc(string) Desc  排序 
5. Get   获取一个
	engine.ShowSQL(true)
	table := goTables{}

	engine.Alias("b").Where("b.id=1").And("name=tb").Get(&table)
	fmt.Printf("table: %v\n", table)

5. Find    查找多个
  	engine.ShowSQL(true)
	// 切片 存储结果
	tb := make([]goTables, 1)

	engine.Asc("id").Find(&tb)
	fmt.Printf("tb: %v\n", tb)

6. ID 主键
	engine.ID(3).Find(&tb)  id=3的
7. Or 或者
8. OrderBy 排序
9. Select(string)  指定查询字段
10. SQL(string)  里面sql语句
```










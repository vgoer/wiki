---
title: 39gorm-crud
description: 
published: 1
date: 2023-09-01T10:32:38.262Z
tags: 
editor: markdown
dateCreated: 2023-09-01T10:32:36.626Z
---

<center>gorm-crud</center>







[toc]







## Gorm-curd

> CRUD通常指数据库的增删改查操作，本文详细介绍了如何使用GORM实现创建、查询、更新和删除操作。

```go
# 1. 创建数据库

# 2. 引入数据模块

package main

import (
	"github.com/jinzhu/gorm"
	_ "github.com/jinzhu/gorm/dialects/mysql"
)

func main() {

	// 创建链接
	db, err := gorm.Open("mysql", "root:root123@(127.0.0.1:3306)/learn_key?charset=utf8mb4&parseTime=True&loc=Local")
	if err != nil {
		panic(err)
	}

	defer db.Close()

}
```





### 1. 创建

> 定义模型

```go
// 模型
type Student struct {
	ID   int64
	Name string
	Age  int64
}
```

使用使用`NewRecord()`查询主键是否存在，主键为空使用`Create()`创建记录：

```go
db.AutoMigrate(&Student{})

stu1 := Student{
    Name: "goer",
    Age:  30,
}

db.NewRecord(stu1) // 主键为空返回`true`
db.Create(&stu1)
db.NewRecord(stu1) // 创建`user`后返回`false`
```



> 设置默认值

```go
type User struct {
  ID   int64
  Name *string `gorm:"default:'小王子'"`
  Age  int64
}
```

> #### 使用指针方式实现零值存入数据库

```go
user := User{Name: new(string), Age: 18))}
db.Create(&user)  // 此时数据库中该条记录name字段的值就是''
```



#### 1. 一般查询

```go
// 根据主键查询第一条记录
db.First(&user)
//// SELECT * FROM users ORDER BY id LIMIT 1;

// 随机获取一条记录
db.Take(&user)
//// SELECT * FROM users LIMIT 1;

// 根据主键查询最后一条记录
db.Last(&user)
//// SELECT * FROM users ORDER BY id DESC LIMIT 1;

// 查询所有的记录
db.Find(&users)
//// SELECT * FROM users;

// 查询指定的某条记录(仅当主键为整型时可用)
db.First(&user, 10)
//// SELECT * FROM users WHERE id = 10;
```



#### 2. Where添加

```go
// Get first matched record
db.Where("name = ?", "jinzhu").First(&user)
//// SELECT * FROM users WHERE name = 'jinzhu' limit 1;

// Get all matched records
db.Where("name = ?", "jinzhu").Find(&users)
//// SELECT * FROM users WHERE name = 'jinzhu';

// <>
db.Where("name <> ?", "jinzhu").Find(&users)
//// SELECT * FROM users WHERE name <> 'jinzhu';

// IN
db.Where("name IN (?)", []string{"jinzhu", "jinzhu 2"}).Find(&users)
//// SELECT * FROM users WHERE name in ('jinzhu','jinzhu 2');

// LIKE
db.Where("name LIKE ?", "%jin%").Find(&users)
//// SELECT * FROM users WHERE name LIKE '%jin%';

// AND
db.Where("name = ? AND age >= ?", "jinzhu", "22").Find(&users)
//// SELECT * FROM users WHERE name = 'jinzhu' AND age >= 22;

// Time
db.Where("updated_at > ?", lastWeek).Find(&users)
//// SELECT * FROM users WHERE updated_at > '2000-01-01 00:00:00';

// BETWEEN
db.Where("created_at BETWEEN ? AND ?", lastWeek, today).Find(&users)
//// SELECT * FROM users WHERE created_at BETWEEN '2000-01-01 00:00:00' AND '2000-01-08 00:00:00';
```












---
title: 38.grom
description: grom
published: 1
date: 2023-05-29T04:13:33.963Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T10:05:32.630Z
---

<center>gorm</center>



[toc]



## GORM

> ORM：对象关系映射， 解决面向对象与关系数据库之间存在不匹配的现象的技术
>
> [blog](https://www.liwenzhou.com/posts/Go/gorm/)
>
> [官方](https://gorm.io/zh_CN/docs/)



### 1. 安装

```go
go get -u gorm.io/gorm
go get -u gorm.io/driver/mysql
```

```php
# 李文周 大佬

go get -u gorm.io/driver/mysql
go get -u github.com/jinzhu/gorm
```





### 2. 连接数据库

> 连接不同的数据库都需要导入对应数据的驱动程序，`GORM`已经贴心的为我们包装了一些驱动程序，只需要按如下方式导入需要的数据库驱动即可：

```php
import _ "github.com/jinzhu/gorm/dialects/mysql"
// import _ "github.com/jinzhu/gorm/dialects/postgres"
// import _ "github.com/jinzhu/gorm/dialects/sqlite"
// import _ "github.com/jinzhu/gorm/dialects/mssql"
```

> #### mysql

```go
import (
  "github.com/jinzhu/gorm"
  _ "github.com/jinzhu/gorm/dialects/mysql"
)

func main() {
    db, err := gorm.Open("mysql", "user:password@(localhost:port)/dbname?charset=utf8mb4&parseTime=True&loc=Local")
  defer db.Close()
}
```

> PostgreSql

```go
import (
  "github.com/jinzhu/gorm"
  _ "github.com/jinzhu/gorm/dialects/postgres"
)

func main() {
  db, err := gorm.Open("postgres", "host=myhost port=myport user=gorm dbname=gorm password=mypassword")
  defer db.Close()
}
```

> sqlite3

```php
import (
  "github.com/jinzhu/gorm"
  _ "github.com/jinzhu/gorm/dialects/sqlite"
)

func main() {
  db, err := gorm.Open("sqlite3", "/tmp/gorm.db")
  defer db.Close()
}
```

> SQL Server

```go
import (
  "github.com/jinzhu/gorm"
  _ "github.com/jinzhu/gorm/dialects/mssql"
)

func main() {
  db, err := gorm.Open("mssql", "sqlserver://username:password@localhost:1433?database=dbname")
  defer db.Close()
}
```







### 3. 使用

> ### 创建数据库

```sql
CREATE DATABASE learn_key
```

> 使用GORM连接上面的`learn_key`进行创建、查询、更新、删除操作。

```go
package main

import (
	"fmt"

	"github.com/jinzhu/gorm"
	_ "github.com/jinzhu/gorm/dialects/mysql"
)

type UserInfo struct {
	ID     uint
	Name   string
	Gender string
	Hobby  string
	Lover  string
}

func main() {

	// 创建链接
	db, err := gorm.Open("mysql", "root:root123@(127.0.0.1:3306)/learn_key?charset=utf8mb4&parseTime=True&loc=Local")
	if err != nil {
		panic(err)
	}
	defer db.Close()

	// 创建迁移
	db.AutoMigrate(&UserInfo{})

	u1 := UserInfo{1, "goer", "男", "篮球", "满"}
	u2 := UserInfo{2, "vuegoer", "女", "足球", "燕"}

	// 创建记录
	db.Create(&u1)
	db.Create(&u2)

	// 查询记录
	var u = new(UserInfo)
	db.First(u)
	fmt.Printf("u: %v\n", u)

	// 条件查询
	var uu UserInfo
	db.Find(&uu, "hobby=?", "足球")
	fmt.Printf("uu: %v\n", uu)

	// 更新
	db.Model(&u).Update("hobby", "双色球")

	// 删除
	db.Delete(&u)

}
```





### 4. GORM Model定义

> 在使用ORM工具时，通常我们需要在代码中定义模型（Models）与数据库中的数据表进行映射，在GORM中模型（Models）通常是正常定义的结构体、基本的go类型或它们的指针。 同时也支持`sql.Scanner`及`driver.Valuer`接口（interfaces）。

> 为了方便模型定义，GORM内置了一个`gorm.Model`结构体。`gorm.Model`是一个包含了`ID`, `CreatedAt`, `UpdatedAt`, `DeletedAt`四个字段的Golang结构体。

```go
// gorm.Model 定义
type Model struct {
  ID        uint `gorm:"primary_key"`
  CreatedAt time.Time
  UpdatedAt time.Time
  DeletedAt *time.Time
}
```

> 你可以将它嵌入到你自己的模型中：

```go
// 将 `ID`, `CreatedAt`, `UpdatedAt`, `DeletedAt`字段注入到`User`模型中
type User struct {
  gorm.Model
  Name string
}

db.AutoMigrate(&UserInfo{})

user := UserInfo{
    Name:  "goer",
    Lover: "蛮子",
}

result := db.Create(&user)

if result.Error != nil {
    panic(result.Error)
} else {
    fmt.Printf("user.Name: %v ,插入成功\n", user.Name)
}
```

> curd:[blog](https://www.liwenzhou.com/posts/Go/gorm-crud/)





### 5. 模型定义

> 自定义模型

```go
type User struct {
  gorm.Model
  Name         string
  Age          sql.NullInt64
  Birthday     *time.Time
  Email        string  `gorm:"type:varchar(100);unique_index"`
  Role         string  `gorm:"size:255"` // 设置字段大小为255
  MemberNumber *string `gorm:"unique;not null"` // 设置会员号（member number）唯一并且不为空
  Num          int     `gorm:"AUTO_INCREMENT"` // 设置 num 为自增类型
  Address      string  `gorm:"index:addr"` // 给address字段创建名为addr的索引
  IgnoreMe     int     `gorm:"-"` // 忽略本字段
}


// 创建迁移
db.AutoMigrate(&User{})
var str string = "goer"

u1 := User{
    Name:         "goer",
    Age:          10,
    Email:        "goer@qq.com",
    Role:         "aa",
    MemberNumber: &str,
    Num:          10,
    Address:      "广东广州",
    IgnoreMe:     1,
}

db.Create(&u1)
fmt.Printf("str is: %v\n", str)
```




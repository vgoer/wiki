---
title: 36.go&mysql
description: go&mysql
published: 1
date: 2023-04-21T10:02:12.861Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T10:02:08.080Z
---

<center>Mysql</center>





[toc]



## Mysql

> 开源免费的数据库软件，目前是oracle的产品



### 1. 下载

> mysql: [mysql](https://dev.mysql.com/downloads/mysql/)

```go
// 连接sql
mysql -u root -p  输入密码

// 创建 数据库
create database go_lang
use go_lang 
// 创建表
create table user ( id INTEGER PRIMARY KEY AUTO_INCREMENT, username VARCHAR(20), password VARCHAR(20));
// 查看表结构
desc user;
// 插入数据
insert into user (username,password) values ("go","golang...");
// 查看数据
select * from user;
// 修改
update user set username="gogo" where username="go";
// 删除
delete from user where username="java";
```

> 更多查看vgoer[blog](https://vgoer.github.io/zh/notes/end/data/mysql/03.sql/#_2-where)



### 2. mysql驱动

> 下载驱动连接数据库 [sql](https://pkg.go.dev/github.com/go-sql-driver/mysql)

```go
// 1.下载驱动
go get -u github.com/go-sql-driver/mysql

// 2.导入包
import (
	"database/sql"
	"time"
	_ "github.com/go-sql-driver/mysql"
)
// 3.连接
func main() {

	// open函数可能只是验证参数格式是否正确，实际不会参与数据库连接
	db, err := sql.Open("mysql", "user:password@/dbname")
	if err != nil {
		panic(err)
	}
	fmt.Println(*db)
	// 最大连接时长
	db.SetConnMaxLifetime(time.Minute * 3)
	// 最大连接数
	db.SetMaxOpenConns(10)
	// 空闲连接数
	db.SetMaxIdleConns(10)
}
```

>  open函数,实际不会参与数据库连接 . 应当调用`ping方法`

```go
package main

import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)

// 定义 指针对象
var db *sql.DB

//初始化数据库函数
func initDb() (err error) {
	// root:123456  用户: 密码    golang => 数据库   
    dns := "root:root@tcp(127.0.0.1:3306)/golang?charset=utf8mb4&parseTime=True"
	// 不会校验账户密码是否正确     这里千万不要  := 重新什么变量了
	db, err = sql.Open("mysql", dns)
	if err != nil {
		return err
	}

	// 尝试与数据库建立连接并校验dns是否正确
	err2 := db.Ping()
	if err2 != nil {
		return err2
	}
	return nil
}
func main() {

	err := initDb()

	if err != nil {
		fmt.Printf("连接失败: %v\n", err)
		return
	} else {
		fmt.Println("数据库连接成功!!")
	}
}
// 这样就连接成功了
```



### 2. 插入

> `db.Exec()` 插入数据

```go
func insertDb(name string, pass string) {

	sqlStr := "insert into user(username,password) values(?,?)"

	r, err := db.Exec(sqlStr, name, pass)
	if err != nil {
		fmt.Printf("err: %v\n", err)
		return
	} else {
		i, _ := r.LastInsertId()
		fmt.Printf("插入的id: %v\n", i)
	}
}
```



### 2. 查询

> 单行 `db.QueryRow()`

```go
// 定义结构体获取数据
type Dbuser struct {
	id       int
	username string
	password string
}

func queryRow(row int) {
	s := "select * from user where id=?"
	var u Dbuser
	// 查询一行 并赋值
	err := db.QueryRow(s, row).Scan(&u.id, &u.username, &u.password)
	if err != nil {
		fmt.Printf("err: %v\n", err)
	} else {
		fmt.Printf("u: %v\n", u)
	}
}
```

> 查询多条 `db.Query()`执行一次查询，返回多行结果（即Rows），

```go
func queryRows() {
	sqlstr := "select * from user"

	rows, err := db.Query(sqlstr)
	if err != nil {
		fmt.Printf("err: %v\n", err)
	}
	// 非常重要：关闭rows释放持有的数据库链接
	defer rows.Close()

	// 循环读取数据
	for rows.Next() {
		var u Dbuser
		err2 := rows.Scan(&u.id, &u.username, &u.password)
		if err2 != nil {
			fmt.Printf("err2: %v\n", err2)
		} else {
			fmt.Printf("获取到数据: %v\n", u)
		}
	}
}
```



### 3.更新

> `db.Exec`

```go
// 修改id所在的行的信息
func update(id int, name string, pass string) {
	sqlstr := "update user 	set username=?, password=? where id=?"

	r, err := db.Exec(sqlstr, name, pass, id)
	if err != nil {
		fmt.Printf("err: %v\n", err)
	} else {
		// 被影响的行
		i, _ := r.RowsAffected()
		fmt.Printf("被影响的行的id: %v\n", i)
	}

}
```



### 4. 删除

> `db.Exec`

```go
// 删除id的信息
func delDb(id int) {
	sqlstr := "delete from user where id=?"
	r, err := db.Exec(sqlstr, id)
	if err != nil {
		fmt.Printf("err: %v\n", err)
	} else {
		// 被印象的行
		i, _ := r.RowsAffected()
		fmt.Printf("删除行id: %v\n", i)
	}
}
```








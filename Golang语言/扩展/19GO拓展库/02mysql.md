---
title: 2.mysql
description: 
published: 1
date: 2023-09-02T03:24:48.832Z
tags: 
editor: markdown
dateCreated: 2023-09-02T03:24:44.053Z
---

<center>mysql</center>



[toc]



## go mysql

> go操作数据库



### 1. mysql

> 开源的关系型数据库

```
1. 下载
https://dev.mysql.com/downloads/mysql/

2. 新建数据库
create database go_db

3. 打开，创建表
use go_db

CREATE TABLE go_language (
  id INTEGER PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR (20),
  pass VARCHAR (255)
)
// id主键  自增

4. 添加数据
INSERT INTO go_language (name,pass) VALUES ("go", "aaa"),("c","bbb"),("java","ccc"),("php","ddd")
```

### 2.安装驱动

> sql驱动库：[mysql-go](https://github.com/go-sql-driver/mysql)

```go
1. 下载包
go get -u github.com/go-sql-driver/mysql

2. 初始化模块
go mod init m(名称)

3.添加需要用到但go.mod中查不到的模块,删除未使用的模块
go mod tidy  

4. 导入驱动
import (
	"fmt"
	"database/sql"
	_ "github.com/go-sql-driver/mysql"  //mysql驱动
)
```



### 3.连接

> 获得连接

```go
package main

import (
	"database/sql"
	_ "github.com/go-sql-driver/mysql"
	"time"
)

func main() {
	db, err := sql.Open("mysql", "root:123456@/go_db")
	if err != nil {
		panic(err)
	}
	print(db)
	// 最大连接时长
	db.SetConnMaxLifetime(time.Minute * 3)
	// 最大连接数
	db.SetMaxOpenConns(10)
	// 空闲连接数
	db.SetMaxIdleConns(10)
}

// 0x11c4a1b0 打印
```

> Open函数可能只是验证其参数格式是否正确，实际上并不创建与数据库的连接。
>
> 如果要检查数据源的名称是否真实有效，应该调用Ping方法。

```go 
// 数据库连接
package main

import (
	"database/sql"
	"fmt"

	_ "github.com/go-sql-driver/mysql"
)

// 定义全局对象db
var db *sql.DB

// 定义初始化数据库函数
func initDB() (err error) {
	// 连接信息 用户 密码  连接地址 端口        数据库  字符编码 时间
	dsn := "root:root123@tcp(127.0.0.1:3306)/go_db?charset=utf8mb4&parseTime=True"
	// 不会校验账号密码是否正确
	// 注意！！！这里不要使用:=，我们是给全局变量赋值，然后在main函数中使用全局变量db
	db, err = sql.Open("mysql", dsn)
	if err != nil {
		return err
	}
	// 尝试与数据库建立连接（校验dsn是否正确）
	err = db.Ping()
	if err != nil {
		return err
	}
	return nil

}

func main() {
	err := initDB() //调用数据库函数
	if err != nil {
		fmt.Printf("初始化失败！,err:%v\n", err)
		return
	} else {
		fmt.Printf("初始化成功")
	}
}

```

### 4. 查询

> 单行查询：
>
> `db.QueryRow`执行一次查询，并期望返回最多一行结果（即Row）。
>
> QueryRow总是返回非nil的值，直到返回值的Scan方法被调用时，才会返回被延迟的错误。

```go
// 1. 定义结构体

type user struct {
	id int
	username string
	password string
}

// 2. 查询
// 单行查询
func queryRowDb() {
	//sql语句 
	sqlStr := "select id,name,pass from go_language where id=?"

	var u user
	// 确保QueryRow之后调用Scan方法，否则持有的数据库链接不会被释放
	err := db.QueryRow(sqlStr, 4).Scan(&u.id, &u.name, &u.pass)
	if err != nil {
		fmt.Printf("查询错误,err:%v\n", err)
		return
	}
	fmt.Printf("id:%d name:%s age:%s\n", u.id, u.name, u.pass)
}

// 3.测试
func main() {
	err := initDB() // 调用输出化数据库的函数
	if err != nil {
		fmt.Printf("初始化失败！,err:%v\n", err)
		return
	}else{
		fmt.Printf("初始化成功")
	}
    // 单条数据查询
	queryRowDemo() 
}
```

> 多条查询
>
> 多行查询`db.Query()`执行一次查询，返回多行结果（即Rows），
>
> 一般用于执行select命令。参数args表示query中的占位参数。

```go
// 查询多条数据示例
func queryMultiRow() {
	sqlStr := "select id, name, pass from go_language"
	rows, err := db.Query(sqlStr)
	if err != nil {
		fmt.Printf("query failed, err:%v\n", err)
		return
	}
	// 非常重要：关闭rows释放持有的数据库链接
	defer rows.Close()

	// 循环读取结果集中的数据
	for rows.Next() {
		var u user
		err := rows.Scan(&u.id, &u.name, &u.pass)
		if err != nil {
			fmt.Printf("scan failed, err:%v\n", err)
			return
		}
		fmt.Printf("id:%d name:%s pass:%s\n", u.id, u.name, u.pass)
	}
}

```

### 5. 插入数据

> 插入、更新和删除操作都使用`Exec`方法。

> `func (db *DB) Exec(query string, args ...interface{}) (Result, error)`

```go

//插入数据
func insertData() {
	sqlStr := "insert into go_language(name,pass) values (?,?)"
	// 插入的数据
	ret, err := db.Exec(sqlStr, "rust", "gggg")
	if err != nil {
		fmt.Printf("插入失败,err:%v\n", err)
		return
	}
	//获取插入数据的id
	theID, err := ret.LastInsertId()
	if err != nil {
		fmt.Printf("获取插入数据id失败,err:%v\n", err)
		return
	}
	fmt.Printf("插入数据成功,this id is %d\n", theID)
}

```

### 6. 删除数据

```go
// 删除数据 
func delData() {
	sqlStr := "delete from go_language where id =?"

	ret, err := db.Exec(sqlStr, "3")
	if err != nil {
		fmt.Printf("删除失败,err%v\n", err)
		return
	}
	//删除的行
	rows, err := ret.RowsAffected()
	if err != nil {
		fmt.Printf("删除行失败,err%v\n", err)
		return
	}
	fmt.Printf("删除成功,删除行：%d.\n", rows)

}
```

### 7. 更新数据

```go
// 更新数据
func updateData() {
	sqlStr := "update go_language set name=?,pass=? where id=?"
	ret, err := db.Exec(sqlStr, "golang", "gogogo", 1)
	if err != nil {
		fmt.Printf("更新失败,err%v\n", err)
		return
	}
	//更新行
	rows, err := ret.RowsAffected()
	if err != nil {
		fmt.Printf("更新行失败,err:%v\n", err)
		return
	}
	fmt.Printf("更新成功,更新行数:%d.\n", rows)
}

```

> ok 数据库就到这里了，赶紧学
















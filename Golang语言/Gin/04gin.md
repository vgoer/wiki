---
title: 04.gin
description: gin
published: 1
date: 2023-04-22T04:47:10.972Z
tags: golang
editor: markdown
dateCreated: 2023-04-22T04:47:08.162Z
---

<center>Gin</center>



[toc]



## Gin

> Gin:[gin](https://gin-gonic.com/)  golang的web框架，基于httpRouter. 



### 1. 安装

> [pack]()

```go
go get -u github.com/gin-gonic/gin
```

```go
// gin 最小应用
import (
	"github.com/gin-gonic/gin"
)

func Hello(c *gin.Context) {
	c.String(200, "hello:%s\n", "goer")
}

func Index(c *gin.Context) {
	c.String(404, "index 首页")
}

func main() {
	server := gin.Default()
	server.GET("/", Index)
	server.GET("/hello", Hello)
	// 默认8080
	server.Run()
}
```



### 2. 登录功能

```go
// 新建 template目录
1. login.html
    <form action="/login" method="post">
        <label for="user">用户名:</label>
        <input type="text" id="user" name="user">
        <br>
        <label for="pass">密码:</label>
        <input type="password" name="pass" id="pass">
        <br>
        <input type="submit" value="登录">
    </form>
2. wecome.html
	// 接收 登录成功返回的页面和值
    <h1>{{.username}}-----{{.password}}</h1>
```

```go
//  服务端
func Login(c *gin.Context) {
	// 跳转到 login.html文件
	c.HTML(200, "login.html", nil)
}

func DoLogin(c *gin.Context) {
	// 接收提交参数  前端请求的name的值
	username := c.PostForm("user")
	password := c.PostForm("pass")
	// gin.H 封装map
	c.HTML(200, "wecome.html", gin.H{
		"username": username,
		"password": password,
	})
}

func main() {
	server := gin.Default()

	// 最短路径下 相对路径
	server.LoadHTMLGlob("template/*")
	server.GET("/login", Login)

	// 提交请求的数据 post
	server.POST("/login", DoLogin)
	// 默认8080
	server.Run(":8989")
}
```






<center>项目之Gin篇</center>



[toc]





### Gin

> Gin项目 [blog](https://blog.csdn.net/jn_however/category_12258302.html)





### 1. 安装

```shell
# 初始化
go mod init vgoer/gin

go get -u github.com/gin-gonic/gin
```

```go
package main

import "github.com/gin-gonic/gin"

func main() {

	// 创建一个默认的路由引擎
	ginServer := gin.Default()

	ginServer.GET("/", func(ctx *gin.Context) {
		ctx.JSON(200, gin.H{"msg": "hello gin", "data": "goer"})
	})

	// 启动 和端口
	ginServer.Run(":9999")
}
```

> 启动`go run main.go`



###  2.路由

> RESTful API 是目前比较成熟的一套互联网应用程序的 API 设计理论 `laravel时学过`

> `资源请求的方法` `GET POST PUT DELETE`

```go
// post 请求
ginServer.POST("/login", func(ctx *gin.Context) {
    ctx.JSON(200, gin.H{"msg": "hello gin", "data": "goer"})
})
```



#### a. 获取get参数

```go
	// http://localhost:9999/news?id=goer
	ginServer.GET("/news", func(ctx *gin.Context) {
		id := ctx.Query("id")
		ctx.String(200, "ID:%s", id)
	})

	// http://localhost:9999/blog/100
	ginServer.GET("/blog/:uid", func(ctx *gin.Context) {

		uid := ctx.Param("uid")
		ctx.String(200, "uid:%s", uid)
	})
```

#### b. 获取post参数

```go
// 获取单个的值
	// 用 apifox  params添加参数
	ginServer.POST("/login", func(ctx *gin.Context) {
		value := ctx.PostForm("name")
        // 当表单当中没有 age ,使用默认值18
		age := c.DefaultPostForm("age", "18")
		ctx.JSON(200, gin.H{
			"value": value,
			"code":  200,
		})
	})
	
// 获取多个的映射
	ginServer.POST("/login", func(ctx *gin.Context) {

		// 获取 Query 参数映射
		params := make(map[string]string)
		fmt.Printf("params: %v\n", params)
		for k, v := range ctx.Request.URL.Query() {
			params[k] = v[0]
		}
		ctx.JSON(200, gin.H{
			"value": hobbies,
			"code":  200,
		})
	})
```



### 3. 路由组

> 路由组和路由文件

```go
// 简单的路由分组

	userRoutes := ginServer.Group("/user")
	{
		userRoutes.GET("/login", func(ctx *gin.Context) {
			ctx.JSON(200, gin.H{
				"msg": "useradd",
			})
		})
	}

	adminRoutes := ginServer.Group("/admin")
	{
		adminRoutes.GET("/login", func(ctx *gin.Context) {
			ctx.JSON(200, gin.H{
				"msg": "admin add",
			})
		})
	}
```

> 路由文件`routes`下创建

```go
// DefaultRoutes.go
package routes

import "github.com/gin-gonic/gin"

func DefaultRoutesInit(r *gin.Engine) {
	defaultRoutes := r.Group("/")
	{
		defaultRoutes.GET("/", func(ctx *gin.Context) {
			ctx.String(200, "首页")
		})
	}
}
```

```go
// AdminRouters
package routes

import "github.com/gin-gonic/gin"

func AdminRoutesInit(r *gin.Engine) {
	adminRoutes := r.Group("/admin")
	{
		adminRoutes.GET("/", func(c *gin.Context) {
			c.String(200, "admin...")
		})
		adminRoutes.GET("/userlist", func(c *gin.Context) {
			c.String(200, "admin---userlist")
		})
		adminRoutes.GET("/plist", func(c *gin.Context) {
			c.String(200, "admin---plist")
		})
	}
}
```

```go
// main.go
func main() {

	// 创建一个默认的路由引擎
	ginServer := gin.Default()
	
	routes.DefaultRoutesInit(ginServer)
	routes.AdminRoutesInit(ginServer)

	// 启动 和端口
	ginServer.Run(":9999")
}

```



### 4. 响应

> 返回的数据

```go
// 字符串
	ginServer.GET("/", func(c *gin.Context) {
		c.String(200, "值: %v", "首页")
	})
```

```go
// 返回json  多种方法
package main

import (
	"github.com/gin-gonic/gin"
)

type Article struct {
	Title   string `json:"title"`
	Desc    string `json:"desc"`
	Content string `json:"content"`
}

func main() {
	ginServer := gin.Default()

	ginServer.GET("/json1", func(c *gin.Context) {
		c.JSON(200, map[string]interface{}{
			"success": true,
			"msg":     "hello gin",
		})
	})
	ginServer.GET("/json2", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"msg": "Gin...",
		})
	})
	ginServer.GET("/json3", func(c *gin.Context) {
		article := &Article{
			Title:   "我是一个标题",
			Desc:    "描述",
			Content: "内容",
		}
		c.JSON(200, article)
	})
	r.Run()
}
```

```go
// xml
	ginServer.GET("/resp", func(ctx *gin.Context) {
		title := &Title{
			Tiele:   "标题是",
			Dosc:    "描述是",
			Content: "内容是",
		}
		ctx.XML(200, title)
	})
// 返回
<Title>
    <Tiele>标题是</Tiele>
    <Dosc>描述是</Dosc>
    <Content>内容是</Content>
</Title>
```

```go
// html 常用

func main() {

	// 创建一个默认的路由引擎
	ginServer := gin.Default()

	// 静态资源的目录
	ginServer.LoadHTMLGlob("templates/*")
	ginServer.GET("/html", func(ctx *gin.Context) {
		ctx.HTML(200, "index.html", gin.H{
			"msg": "我是首页返回的值",
		})
	})

	// 启动 和端口
	ginServer.Run(":9999")
}


// templates 下新建 index.html  获取路由传来的值
    <h1>{{.msg}}</h1>
```



### 5. 模版

>  templates 文件  `定义模板的时候需要通过 define 定义名称` 重要

```go
// templates/admin/index.html
<!--相当于给模板定义一个名字 define end 成对出现-->
{{ define "admin/index.html"}}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>admin</title>
</head>
<body>
    <h2>{{.title}}</h2>
</body>
</html>
{{end}}
```

```go
// 路由中引入 AdminRoutes.go
func AdminRoutesInit(r *gin.Engine) {
	// admin 下面的文件
	r.LoadHTMLGlob("templates/admin/*")
	adminRoutes := r.Group("/admin")
	{
		adminRoutes.GET("/", func(ctx *gin.Context) {
			ctx.HTML(200, "admin/index.html", gin.H{"title": "yes"})
		})
	}
}

// mian.go中引入
routes.AdminRoutesInit(ginServer)
```

> 打印 数据 ` if  Range With 自定义模板函数`这里就不说了

> 嵌套的`template`

```html
{{ define "public/page_header.html" }}
<h1>这是一个头部</h1>
{{end}}
```

```html
引入 引入的时候注意到最后的点（.）
{{template "public/page_header.html" .}}
```

> 静态资源`static`

```go
r.Static("/static","./static") 第一个参数表示路由，第二个参数表示路径

// 当目录下不存 index.html 文件时，会列出该目录下的所有文件。
// ginServer.StaticFS("/static", http.Dir("static"))
```



### 6. 控制器和中间件

> 先前我们的业务都在路由里面，分开放到控制器里面

```go
// controller/admin/adminController.go
package admin

import "github.com/gin-gonic/gin"

type AdminController struct {
}

func (admin AdminController) Admin(ctx *gin.Context) {
	ctx.String(200, "后台页面")
}

func (admin AdminController) List(ctx *gin.Context) {
	ctx.String(200, "管理员列表")
}
```

```go
// AdminRouters.go
package routes

import (
	"vgoer/gin/controller/admin"

	"github.com/gin-gonic/gin"
)

func AdminRoutesInit(r *gin.Engine) {

	// admin 下面的文件
	r.LoadHTMLGlob("templates/admin/*")
	adminRoutes := r.Group("/admin")
	{
		adminRoutes.GET("/", admin.AdminController{}.Admin)
		adminRoutes.GET("/list", admin.AdminController{}.List)
	}
}
```



#### a.控制器继承

```go
// controller/admin/baseController.go
package admin

import "github.com/gin-gonic/gin"

type BaseController struct {
}

func (con BaseController) success(c *gin.Context) {
	c.String(200, "成功")

}
func (con BaseController) error(c *gin.Context) {
	c.String(200, "失败")
}
```

```go
type AdminController struct {
	BaseController
}

func (admin AdminController) Admin(ctx *gin.Context) {
	// ctx.String(200, "后台页面")
	admin.success(ctx)
}
```

#### b.中间件

> 中间件就是匹配路由前和匹配路由完成后执行的一系列操作

```go
// 路由中间件 最后一个 func 回调函数前面触发的函数都可以称为中间件。
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
)

// 这个就是中间件
func InitMessage(ctx *gin.Context) {
	id := 1
	fmt.Printf("id: %v\n", id)
}

func main() {
	// 创建一个默认的路由引擎
	ginServer := gin.Default()
	ginServer.GET("/", InitMessage, func(ctx *gin.Context) {
		ctx.String(200, "yes")
	})
	// 启动 和端口
	ginServer.Run(":9999")
}
```

> 中间件中加上 c.Next() 可以让我们在路由匹配完成后执行一些操作
>
> 一个路由，多个控制器，执行顺序

```go
package main

import (
	"fmt"

	"github.com/gin-gonic/gin"
)

func InitMiddlewareOne(c *gin.Context) {
	fmt.Println("1")
	// 调用该请求的剩余处理程序
	c.Next()
	fmt.Println("11")

}

func InitMiddlewareTwo(c *gin.Context) {
	fmt.Println("2")
	// 调用该请求的剩余处理程序
	c.Next()
	fmt.Println("22")

}
func main() {
	r := gin.Default()
	r.GET("/", InitMiddlewareOne, InitMiddlewareTwo, func(c *gin.Context) {
		fmt.Println("首页...")
		c.String(200, "首页")
	})
	r.GET("/news", InitMiddlewareOne, InitMiddlewareTwo, func(c *gin.Context) {
		c.String(200, "新闻")
	})

	r.Run(":9999")
}

```

```shell
1
2
首页...
22
11
```

> 中间件文件

```go
// middlewares/InitMiddleware.go
package middlewares

import (
	"fmt"

	"github.com/gin-gonic/gin"
)
func InitMiddlewareOne(c *gin.Context) {
	fmt.Println("1")
	// 调用该请求的剩余处理程序
	c.Next()
	fmt.Println("11")

}

func InitMiddlewareTwo(c *gin.Context) {
	fmt.Println("2")
	// 调用该请求的剩余处理程序
	c.Next()
	fmt.Println("22")

}
```

> 使用

```go
func AdminRoutesInit(r *gin.Engine) {

	// admin 下面的文件
	r.LoadHTMLGlob("templates/admin/*")
	adminRoutes := r.Group("/admin", middlewares.InitMiddlewareOne, middlewares.InitMiddlewareTwo)
	{
		adminRoutes.GET("/", admin.AdminController{}.Admin)
		adminRoutes.GET("/list", admin.AdminController{}.List)
	}
}

```
















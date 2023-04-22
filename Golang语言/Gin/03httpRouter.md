---
title: 03.httpRouter
description: http路由
published: 1
date: 2023-04-22T04:45:52.305Z
tags: golang
editor: markdown
dateCreated: 2023-04-22T04:45:52.305Z
---

<center>httpRouter</center>



[toc]





## httpRouter

> 轻量级高性能的http请求路由器



### 1. 安装

> [router](https://pkg.go.dev/github.com/julienschmidt/httprouter)

```go
// 如果太慢设置国内源
go env -w GOPROXY=https://goproxy.cn,direct
go get github.com/julienschmidt/httprouter
```

```go
func Index(w http.ResponseWriter, r *http.Request, _ httprouter.Params) {
	fmt.Fprintf(w, "index\n")

}
func Hello(w http.ResponseWriter, r *http.Request, ps httprouter.Params) {
	fmt.Fprintf(w, "hello, %s\n", ps.ByName("name"))
}

func main() {

	router := httprouter.New()
	router.GET("/", Index)
	router.GET("/hello/:name", Hello)

	log.Fatal(http.ListenAndServe(":7878", router))
}
```


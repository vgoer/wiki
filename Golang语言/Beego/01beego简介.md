---
title: 01.beego简介
description: beego简介
published: 1
date: 2023-06-09T10:11:26.722Z
tags: golang
editor: markdown
dateCreated: 2023-04-22T04:54:49.905Z
---

<center>Beego</center>



[toc]



## Beego

> beego 是免费、开源的软件 ,快速开发GO应用的http框架
>
> [beego文档](https://beego.vip/quickstart)



### 1. 安装

> 安装了bee但是找不到 [bee](https://blog.csdn.net/qq_32421489/article/details/124610661)

```go
// 打开go mode 
go env -w GO111MODULE=on
// $GOPATH/bin 加入到你的 $PATH 变量中
添加环境变量

// 初始化
go mod init geegoer
// 安装
go get -u github.com/beego/beego/v2
go get -u github.com/beego/bee/v2
// 依赖关系处理
go mod tidy
```

> `bee` 相当于 `npm`

```go
bee new gogee     //新建项目
go mod tidy       //安装依赖
bee run           // 跑起来

启动了，It's coll 
```



### 2.项目结构

> bee 项目

```go
conf  配置文件
controllers 控制器
models 模型
routers 路由文件
views 模板文件
.tpl 模板文件
// 获取到控制器内定义的
网站:{{.Website}}
名称:{{.Name}}
```



### 3. bee

> 管理 beego工具

```go
bee new 项目名称 // 新建前后端不分离的项目

// api 创建API   创建也前后端分离的项目
bee api 项目名称  

// pack 打包 
bee pack    //打包压缩文件
```








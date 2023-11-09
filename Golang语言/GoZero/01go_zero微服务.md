<center>Go_zero</center>







[toc]







## Go_zero

>  go-zero 是一个集成了各种工程实践的 web 和 rpc 框架。 [go_zero](https://go-zero.dev/)
>
> 快速开始： [go_zero快速开始](https://go-zero.dev/docs/tasks)









### 1. 安装 Golang

> Go 是 Google 开发的一种静态强类型、编译型、并发型，并具有垃圾回收功能的编程语言.
>
> [go install](https://go-zero.dev/docs/tasks)

```go
go version


// win平台 其他平台查看文档
go version go1.21.1 windows/amd64
```



> 配置go

#### a.GO111MODULE 开启

> 在 go 1.11 以后建议将 `GO111MODULE` 值显式设置为 `on`，以免后续拉取依赖出现一些不必要的错误。

```go
go env -w GO111MODULE=on
```



#### b. 配置Proxy

> 配置代理

```go
go env -w GOPROXY=https://goproxy.cn,direct
```



> 查看配置

```go
go env GO111MODULE

go env
```









### 2. goctl 安装

> goctl 是 go-zero 的内置脚手架，是提升开发效率的一大利器，可以一键生成代码、文档、部署 k8s yaml、dockerfile 等。



> 1. 查看版本

```go
go version
```

> 2.  go[get, install]

- 如果 go 版本在 `1.16` 以前，则使用如下命令安装：

  ```bash
  GO111MODULE=on go get -u github.com/zeromicro/go-zero/tools/goctl@latest
  ```

- 如果 go 版本在 `1.16` 及以后，则使用如下命令安装：

  ```bash
  GO111MODULE=on go install github.com/zeromicro/go-zero/tools/goctl@latest
  ```



> 3. 验证

```go
goctl --version
```







### 3. protoc安装

> [protoc](https://protobuf.dev/) 是一个用于生成代码的工具，它可以根据 proto 文件生成C++、Java、Python、Go、PHP 等多重语言的代码，而 gRPC 的代码生成还依赖 [protoc-gen-go](https://github.com/golang/protobuf/tree/master/protoc-gen-go)，[protoc-gen-go-grpc](https://pkg.go.dev/google.golang.org/grpc/cmd/protoc-gen-go-grpc) 插件来配合生成 Go 语言的 gRPC 代码。



> 1. 一键安装（推荐）

```go
goctl env check --install --verbose --force
```

> 2. 验证

```go
goctl env check --verbose

[goctl-env]: preparing to check env

[goctl-env]: looking up "protoc"
[goctl-env]: "protoc" is installed

[goctl-env]: looking up "protoc-gen-go"
[goctl-env]: "protoc-gen-go" is installed

[goctl-env]: looking up "protoc-gen-go-grpc"
[goctl-env]: "protoc-gen-go-grpc" is installed

[goctl-env]: congratulations! your goctl environment is ready!
```





### 4. 编辑器插件

> vscode: goctl-vscode  => goctl 
>
> intellij: Goctl






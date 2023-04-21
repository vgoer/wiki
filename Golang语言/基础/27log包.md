---
title: 27.log包
description: log包
published: 1
date: 2023-04-21T09:51:16.151Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T09:51:12.316Z
---

<center>log</center>





[toc]



## log库

> 日志库 



### 1. 使用

> log包有三个系列的日志函数

| 函数系列 | 作用                                              |
| :------- | :------------------------------------------------ |
| print    | 单纯打印                                          |
| panic    | 打印日志，抛出panic异常                           |
| fatal    | 打印日志，强制结束程序os.Exit(1), defer函数不执行 |

```go
// print
log.Print("this is log") //2022/07/30 14:48:36 this is log
```

```go
// printf 
// 格式化输出
log.Printf("my log %d", 100)
```

```go
// println
name := "gom"
age := 10
// 输出格式
log.Println("我", name, "今年", age, "岁了")
```

> panic

```go
func main() {
	log.Print("print ...")
	log.Panic("抛出异常了")
	defer fmt.Println("打印我")
}
// 抛出异常 
```

```go
// fatal
log.Fatal("fatal...")  // 退出程序了
```



### 2.配置打印格式

> 

```go
const (
	Ldate         = 1 << iota     // the date in the local time zone: 2009/01/23
	Ltime                         // the time in the local time zone: 01:23:23
	Lmicroseconds                 // microsecond resolution: 01:23:23.123123.  assumes Ltime.
	Llongfile                     // full file name and line number: /a/b/c/d.go:23
	Lshortfile                    // final file name element and line number: d.go:23. overrides Llongfile
	LUTC                          // if Ldate or Ltime is set, use UTC rather than the local time zone
	Lmsgprefix                    // move the "prefix" from the beginning of the line to before the message
	LstdFlags     = Ldate | Ltime // initial values for the standard logger
)
```

```go
// 打印格式 SetFlags 
func main() {
	log.Print("my log...")
}

func init() {
	log.SetFlags(log.Ldate | log.Ltime | log.Llongfile)
}
```

```go
// 设置前缀 SetPrefix
func init() {
	log.SetFlags(log.Ldate | log.Ltime | log.Llongfile)
	log.SetPrefix("my log: ")
}
```

```go
// 指定写入的文件 SetOutput
func init() {
	log.SetFlags(log.Ldate | log.Ltime | log.Llongfile)
	log.SetPrefix("my log: ")
	f, err := os.OpenFile("a.log", os.O_RDWR|os.O_APPEND|os.O_CREATE, 0664)

	if err != nil {
		log.Fatal("日志文件错误")
	}
	// 日志保存到文件中
	log.SetOutput(f)
}
```

```go
// 整合以上功能的 logger
var loger *log.Logger

func init() {
	f, err := os.OpenFile("a.log", os.O_RDWR|os.O_APPEND|os.O_CREATE, 0664)

	if err != nil {
		log.Fatal("日志文件错误")
	}
	// 日志保存到文件中
	log.SetOutput(f)

	loger = log.New(f, "log fire: ", log.Ldate|log.Ltime|log.Llongfile)
}

func main() {
	loger.Print("my log...")
}

```


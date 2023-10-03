---
title: 32.time包
description: time包
published: 1
date: 2023-06-09T10:12:27.055Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T09:57:16.586Z
---

<center>time包</center>



[toc]



## time包

> 提供时间的功能函数

```go
func timeDemo() {
    now := time.Now() //获取当前时间
    fmt.Printf("current time:%v\n", now)

    year := now.Year()     //年
    month := now.Month()   //月
    day := now.Day()       //日
    hour := now.Hour()     //小时
    minute := now.Minute() //分钟
    second := now.Second() //秒
    fmt.Printf("%d-%02d-%02d %02d:%02d:%02d\n", year, month, day, hour, minute, second)
}
```

```go
// 当前时间
t := time.Now()
y := t.Year()
m := t.Month()
fmt.Printf("t: %v\n", t) // 当前时间
```

```go
// 时间戳
now := time.Now()
i := now.Unix()
fmt.Printf("i: %v\n", i)
```



### 操作时间

```go
// add
func main() {
    now := time.Now()
    later := now.Add(time.Hour) // 当前时间加1小时后的时间
    fmt.Println(later)
}
```

```go
// sub
func main() {
    now := time.Now()
    later := now.Sub(time.Hour) // 当前时间减去1小时后的时间
    fmt.Println(later)
}
```

```go
// 定时器
func tickDemo() {
    ticker := time.Tick(time.Second) //定义一个1秒间隔的定时器
    for i := range ticker {
        fmt.Println(i)//每秒都会执行的任务
    }
}
```

> 格式化： 不是常见的Y-m-d H:M:S而是使用Go的诞生时间`2006年1月2号15点04分（记忆口诀为2006 1 2 3 4）`

```go
func formatDemo() {
    now := time.Now()
    // 格式化的模板为Go的出生时间2006年1月2号15点04分 Mon Jan
    // 24小时制
    fmt.Println(now.Format("2006-01-02 15:04:05.000 Mon Jan"))
    // 12小时制
    fmt.Println(now.Format("2006-01-02 03:04:05.000 PM Mon Jan"))
    fmt.Println(now.Format("2006/01/02 15:04"))
    fmt.Println(now.Format("15:04 2006/01/02"))
    fmt.Println(now.Format("2006/01/02"))
}
```






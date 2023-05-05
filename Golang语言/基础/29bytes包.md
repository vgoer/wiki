---
title: 29.bytes包
description: bytes包
published: 1
date: 2023-05-05T11:47:38.144Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T09:53:46.302Z
---

<center>bytes包</center>





[toc]



## bytes包

> 提供对 ==字节切片==的读写操作的函数
>
> 字节单个字符， 可以和字符串轻松转换

```go
// 强制转换 
var i int = 10
var b byte = 1
j := int(b)
t := byte(i)
fmt.Printf("i: %T\n", i) // int
fmt.Printf("j: %T\n", j) // int
fmt.Printf("t: %T\n", t) // uint8
```

```go
var s string = "golang"
// 字符串转化为切片
ss := []byte(s)
fmt.Printf("ss: %v\n", ss) //  [103 111 108 97 110 103]
// 字节切片转换为字符串
fmt.Printf("string(ss): %v\n", string(ss))
```



### 1. 常用函数

```go
// 是否包含
bt := []byte("hello")

b1 := []byte("he")
b2 := []byte("eh")
// 是否包含字符
fmt.Println(bytes.Contains(bt, b1)) // true
fmt.Println(bytes.Contains(bt, b2)) // false
```

```go
// 字符出现的次数
bt := []byte("hello")

b1 := []byte("l")

fmt.Println(bytes.Count(bt, b1)) // 2
```

```go
// 重复打印这个字符几次
bt := []byte("hello")
    // 重复打印这个字符几次
    fmt.Println(string(bytes.Repeat(bt, 3))) // hellohellohello
}
```

```go
// 替换字符
bt := []byte("hello")

old := []byte("l")
new := []byte("LL")
// -1 无线次数  1 替换一次
fmt.Println(string(bytes.Replace(bt, old, new, -1)))
```

```go
// 汉字计算len
s := []byte("我是")
// 一个汉字 3字节
fmt.Printf("len(s): %v\n", len(s)) // 6
r := bytes.Runes(s)
fmt.Printf("len(r): %v\n", len(r)) //2
```

```go
// join  切片连接
s := [][]byte{[]byte("你好"), []byte("世界")}

sep := []byte("+")
fmt.Println(string(bytes.Join(s, sep))) //你好+世界
```



### 2.Reader类型

> 实现了io里面的一些接口

```go
```



### 3. Buffer类型

> 可变大小的字节缓冲区

```go
```












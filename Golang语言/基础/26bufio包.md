---
title: 26.bufio包
description: bufio包
published: 1
date: 2023-06-09T10:12:17.194Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T09:49:44.337Z
---

<center>bufio</center>



[toc]





## bufio

> 实现了有缓存的io  封装了io.Reader或io.Writer接口

```go
// 读文件和字符
// 字符reader
// r := strings.NewReader("hi golang")

// 文件
f, _ := os.Open("a.txt")
defer f.Close()
r2 := bufio.NewReader(f)

// 读到换行
s, _ := r2.ReadString('\n')
fmt.Printf("s: %v\n", s)
```

```go
// 读一行 ReadLine
s := strings.NewReader("abc\nbbb\nccccc\n")

buf := bufio.NewReader(s)

line, isPrefix, _ := buf.ReadLine()

fmt.Printf("line: %v %v\n", string(line), isPrefix)

line, isPrefix, _ = buf.ReadLine()
fmt.Printf("line: %v %v\n", string(line), isPrefix)
```

```go
// ReadSlice('.') 读取一个切片
s := strings.NewReader("abc,bbb,ccccc,")

buf := bufio.NewReader(s)
// 分割符
line, _ := buf.ReadSlice(',')
fmt.Printf("line: %q\n", line)
```

```go
// ReadBytes 读字节  分割开 同上一样
s := strings.NewReader("abc,bbb,ccccc,")

buf := bufio.NewReader(s)

b, _ := buf.ReadBytes(',')
fmt.Printf("b: %q\n", b)
```

```go
// ReadString 读字符串 同上一样
s := strings.NewReader("abc,bbb,ccccc,")

buf := bufio.NewReader(s)

b, _ := buf.ReadString(',')
fmt.Printf("b: %q\n", b)
```

> io的写入

```go
// 写入文件
s := strings.NewReader("hello world")

// 建一个缓冲区
buf := bufio.NewReader(s)

f, _ := os.OpenFile("c.txt", os.O_RDWR|os.O_APPEND|os.O_CREATE, 0777)
defer f.Close()

// 缓冲区写入到 文件
buf.WriteTo(f)
```

```go
// NewWriter 来写入
f, _ := os.OpenFile("a.txt", os.O_RDWR, 0777)

defer f.Close()

// 写入的缓冲区
w := bufio.NewWriter(f)
w.WriteString("golang yes")
// 没有写入  应该刷新缓冲区
w.Flush()
w.Reset(c) // 清除缓冲区
```

```go
// 写入 WriteBtye('') 写入字符 int8
//     WriteRune('') 写入汉子 int32
```



### Scanner

> 提供更方便大的读取数据的接口

```go
// 读取字符和文件
s := strings.NewReader("aaa bbb ccc ddd eee")

bs := bufio.NewScanner(s)

// 以一个空格分割
bs.Split(bufio.ScanWords)
// 循环出来
for bs.Scan() {
    fmt.Println(bs.Text())
}
```

```go
// 以一个字母分割
bs.Split(bufio.ScanBytes)
```

```go
// 以一个字母汉子分割
bs.Split(bufio.ScanRunes)
```


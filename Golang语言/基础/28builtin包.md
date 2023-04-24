---
title: 28.builtin包
description: builtin包
published: 1
date: 2023-04-24T10:30:49.663Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T09:52:50.836Z
---

<center>builtin包</center>



[toc]





## builtin

> 提供了一些类型申明，变量和常量申明 一些常用函数
>
> > 这个包不需要导入

```go
// append  切片后追加
var sli = []int{1, 2}

// 切片
i := append(sli, 100) // i: [1 2 100]

// // 切片添加切片
i2 := append(sli, i...)
fmt.Printf("i2: %v\n", i2) // [1 2 1 2 100]
```

```go
// len() 获取长度
i3 := len(sli)
fmt.Printf("i3: %v\n", i3)
```

```go
// print println 打印不换行和换行
print(name, " ", name, "\n")
println(name, " ---")
```

```go
// panic 抛出异常程序结束  defer也可以执行
panic("结束了")
print(name, " ", name, "\n")
```

```go
重要 new make 
var p *[]int = new([]int)
fmt.Printf("p: %v\n", p) // p: &[] 指针地址

var v = make([]int, 10)
fmt.Printf("v: %v\n", v) // v: [0 0 0 0 0 0 0 0 0 0]  初始化了
```

1. make 只能分配和初始化的类型为`slice,map,chan`的数据  new 可以分配任意类型
2. new 分配返回的是指针 类型为*T, make 返回引用类型 T
3. new 分配的空间被清零，make 分配后会初始化

```go
// new
in := new(int)
fmt.Printf("in: %T\n", in)  // 指针类型 *int
fmt.Printf("in: %v\n", in)  // 地址0xc000016098
fmt.Printf("in: %v\n", *in) // *i 取值  0
```

```go
// make
arr := [3]int{1, 2, 3}

fmt.Printf("arr: %T\n", arr) // [3]int 数组int
fmt.Printf("arr: %v\n", arr) 
```


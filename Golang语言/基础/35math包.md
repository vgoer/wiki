---
title: 35.math包
description: math包
published: 1
date: 2023-04-29T17:44:22.951Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T10:00:16.082Z
---

<center>math包</center>



[toc]





## math包

> 一些常量和计算函数



### 1. 常量

```go
一些常量的最大值最小值  
math.MaxInt int的最大值
math.MinInt int的最小值
math.Pi  圆周率
```



### 2. 函数

> 大部分语言都差不多

```go
Abs 绝对值
Pow x的y次方
Pow10() 10的几次方
Sqrt() 开平方
Cbrt() 开立方
Ceil() 向上取整
Floor() 向下取整
Mod()  取余
```

```go
// 随机数
// 种子要进行设置  不然第二次和第一次产生的一样
for i := 0; i < 10; i++ {
    a := rand.Int()
    fmt.Printf("a: %v\n", a)
}
// 两次循环的一样
```

```go
// 防止以上情况
func init() {
	// 随机种子
	rand.Seed(time.Now().UnixMicro())
}

func main() {
	for i := 0; i < 10; i++ {
		a := rand.Int()
		fmt.Printf("a: %v\n", a)
	}
}
```

```go
// intn() 以内的随机数
a := rand.Intn(10)

// 小数的随机数
a := rand.Float32()
```




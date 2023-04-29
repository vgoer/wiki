---
title: 15.map
description: map
published: 1
date: 2023-04-29T17:43:35.890Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T09:29:14.930Z
---

<center>Map</center>



[toc]







## Map

> 内部使用`散列表（hash）`实现。

> map是一种无序的基于`key-value`的数据结构，Go语言中的map是引用类型，必须初始化才能使用。

```go
map[KeyType]ValueType
make(map[KeyType]ValueType, [cap])
```

```go
func setMap() {

	m1 := map[string]string{
		"name": "golang",
		"age":  "30",
		"num":  "1111",
	}

	m2 := make(map[string]string, 30)
	m2["name"] = "java"
	m2["age"] = "30"

	fmt.Printf("m1: %v\n", m1)
	fmt.Printf("m2: %v len:%v\n", m2, len(m2))
}
```



### 1. 判断值存在

> go的特殊写法 

```go
value, ok := map[key]  //ok:bool

func valueTure() {
	m1 := map[string]string{
		"name": "golang",
		"age":  "30",
		"num":  "1111",
	}
	value, ok := m1["age"]
	fmt.Printf("v: %v\n", value)  // 30
	fmt.Printf("ok: %v\n", ok)    // true
}
```



### 2. 遍历

> map是无序的

```go
func forMap() {
	var m1 = map[string]string{"name": "tom", "age": "100"}
	for k, v := range m1 {
		fmt.Printf("%v: %v\n", k, v)
	}
}
```



### 3. 操作

> 增删改查

```go
// 删除  delete(map, key)

var m1 = map[string]string{"name": "tom", "age": "100"}
// 删除
delete(m1, "name")

// 添加
m1["love"] = "篮球"

// 改 查 都是用 键获取修改
fmt.Printf("m1: %v\n", m1)
```




















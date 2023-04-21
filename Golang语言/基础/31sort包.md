---
title: 31.sort包
description: sort包
published: 1
date: 2023-04-21T09:55:56.746Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T09:55:56.746Z
---

<center>sort包</center>



[toc]



## sort包

> 提供了排序切片和用户自定义数据集的函数



```go
// 排序 Ints
s := []int{10, 2, 3, 4}

sort.Ints(s)
fmt.Printf("s: %v\n", s) //[2 3 4 10]
```


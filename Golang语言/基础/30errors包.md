---
title: 30.errors包
description: errors包
published: 1
date: 2023-06-09T10:12:22.836Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T09:54:57.859Z
---

<center>errors包</center>



[toc]





## errors包

> 实现了操作错误的函数

```go
func check(s string) (string, error) {

	if s == "" {
		// 新建错误
		err := errors.New("字符串不能为空")
		return "", err
	} else {
		return s, nil
	}
}
func main() {
	s, err := check("")
	if err != nil {
		fmt.Printf("err: %v\n", err.Error())
	} else {
		fmt.Printf("s: %v\n", s)
	}
}
```



### 自定义错误

> 自定义

```go
// 官方 https://pkg.go.dev/errors@go1.18.4


```


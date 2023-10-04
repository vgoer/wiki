---
title: 4.input
description: 
published: 1
date: 2023-09-02T03:25:07.548Z
tags: 
editor: markdown
dateCreated: 2023-09-02T03:25:00.351Z
---

<center>input</center>



[toc]



## 输入

> 命令行获取输入字符



>  1.获取unicode字符

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)
func main() {
	reader := bufio.NewReader(os.Stdin)

	char, _, err := reader.ReadRune()

	if err != nil {
		fmt.Println("错误:", err)
	}

	// 打印 unicode值
	fmt.Println(char)2

}
```



> 2.读取整个句子
>
> 每次输入一个字符串并按下 `enter` 键，我们会通过 `\n` 这个关键字符来区分字符串

```go
func main() {
	reader := bufio.NewReader(os.Stdin)

	fmt.Println("-------输入-------")

	// 相当于while
	for {
		fmt.Print("---->")
		text, _ := reader.ReadString('\n')

		// windows  strings.Replace(text,"\r\n","",-1)
		text = strings.Replace(text, "\n", "", -1)
		fmt.Println(text)

		if text == "q" {
			break
		}
	}
}
```

> windows系统 `\r\n`换行



> 3. bufio，权威的方法

```go
func scanner() {
	scanner := bufio.NewScanner(os.Stdin)
	for scanner.Scan() {
		fmt.Println(scanner.Text())
	}
}

```

> `os.Exit`立即==退出并返回给定状态==。

```go
os.Exit(1)   //退出状态1
```



> 生成一个随机数

```go
// 随机种子
rand.Seed(time.Now().Unix())
// 生成 20 个 [0, 100) 范围的伪随机数。
result := rand.Intn(100)
fmt.Println(result)
```










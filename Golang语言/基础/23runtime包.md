---
title: 23.runtime包
description: runtime包
published: 1
date: 2023-04-29T17:43:52.473Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T09:46:00.880Z
---

<center>runtime包</center>





[toc]



## runtime包

> 定义了一些协程管理相关的api



### 1. Gosched()

> 让出cpu时间片，重新等待安排任务

```go
// 我有权利执行，但我让你先执行
//排队拿快递，到我了，但是后面有人比较急，我让他先拿

func showMsg(msg string) {

	for i := 0; i < 3; i++ {
		fmt.Printf("msg: %v\n", msg)
	}
}
func main() {

	go showMsg("golang") //创建子协程

	// 主协程
	for i := 0; i < 4; i++ {

		runtime.Gosched() // 我有权利先执行，但我让你先执行
		fmt.Println("javascript")
	}

	fmt.Println("主协程 end ....")
}

```



### 2. Goexit()

> 退出当前协程

```go
func showMsg() {

	for i := 0; i <= 3; i++ {

		fmt.Printf("i: %v\n", i)
		if i == 2 {
			// 退出协程
			runtime.Goexit()
		}
	}
}
func main() {

	go showMsg() //创建子协程
	time.Sleep(time.Second)

}
```



### 3. GOMAXPROCS()

> cup 核心数  `NumCPU` -> 获取到cpu的核心数

```go
func a() {
	for i := 0; i < 20; i++ {
		fmt.Printf("a: %v\n", i)
	}
}

func b() {
	for i := 100; i < 110; i++ {
		fmt.Printf("b: %v\n", i)
	}
}

func main() {
	// NumCPU cup核心数
	numcpu := runtime.NumCPU()
	// 让空闲的cup也参与进来
	runtime.GOMAXPROCS(2)
	go a()
	go b()

	time.Sleep(time.Second)

	fmt.Printf("numcpu: %v\n", numcpu)
}
```








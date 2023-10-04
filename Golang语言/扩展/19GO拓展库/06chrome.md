---
title: 06chrome
description: 
published: 1
date: 2023-09-03T10:29:55.687Z
tags: 
editor: markdown
dateCreated: 2023-09-02T03:25:10.078Z
---

<center>Chrome</center>





[toc]









### Chrome

> go 对 谷歌浏览器的操作







### 1.打开谷歌

> 在 Go 中，你可以使用 `os/exec` 包来执行操作系统的命令，从而模拟打开浏览器，并登录网站。

```go
package main

import (
	"os"
	"os/exec"
	"runtime"
)

// 打开浏览器
func openBrowser(url string) error {
	var cmd *exec.Cmd

	switch runtime.GOOS {
	case "darwin":
		cmd = exec.Command("open", url)
	case "windows":
		cmd = exec.Command("cmd", "/c", "start", url)
	default:
		cmd = exec.Command("xdg-open", url)
	}

	return cmd.Start()

}

func main() {
	url := "https://manage.baotong-soft.com/"

	err := openBrowser(url)
	if err != nil {
		os.Exit(1)
	}
}
```



### 2. 另一种登录

> 展示了如何使用 `Agouti` 库在网页中找到用户名和密码输入框 [chromedriver](https://www.jianshu.com/p/dc0336a0bf50)

```go
package main

import (
	"log"
	"time"

	"github.com/sclevine/agouti"
)

func main() {
	// 创建一个新的 WebDriver
	// driver := agouti.ChromeDriver(agouti.ChromeOptions("args", []string{
	// 	"--headless",       // 在后台运行 Chrome
	// 	"--no-sandbox",
	// 	"--disable-gpu",
	// }))
	driver := agouti.ChromeDriver()

	// 启动 WebDriver 会话
	err := driver.Start()
	if err != nil {
		log.Fatal("无法启动 WebDriver:", err)
	}

	// 打开目标网页
	page, err := driver.NewPage()
	if err != nil {
		log.Fatal("无法打开网页:", err)
	}
	err = page.Navigate("https://xx.xx-xx.com/")
	if err != nil {
		log.Fatal("无法访问网页:", err)
	}

	// 使用 CSS 选择器查找用户名和密码输入框
	// usernameInput := page.FindByID("username")
	// passwordInput := page.FindByID("password")

	usernameInput := page.FindByName("username")
	passwordInput := page.FindByName("password")

	// 输入登录信息
	err = usernameInput.Fill("admin")
	if err != nil {
		log.Fatal("无法填写用户名:", err)
	}

	err = passwordInput.Fill("admin123...")
	if err != nil {
		log.Fatal("无法填写密码:", err)
	}

	time.Sleep(5 * time.Second)
	
    // 提交登录表单（可选）
	button := page.FindByButton("登录") // 根据按钮文本查找按钮元素
	if button == nil {
		log.Fatalf("找不到按钮")
	}

	err = button.Click() // 点击按钮
	if err != nil {
		log.Fatalf("无法点击按钮：%v", err)
	}
	// 提交登录表单（可选）
	//err = passwordInput.Submit()
	//if err != nil {
	//	log.Fatal("无法提交表单:", err)
	//}

	// 关闭 WebDriver 会话
	// err = driver.Stop()
	// if err != nil {
	// 	log.Fatal("无法停止 WebDriver:", err)
	// }
}

```


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
	"fmt"
	"log"

	"github.com/sclevine/agouti"
)

func main() {
	// 创建一个新的 WebDriver
	driver := agouti.ChromeDriver(agouti.ChromeOptions("args", []string{
		"--headless",       // 在后台运行 Chrome
		"--no-sandbox",
		"--disable-gpu",
	}))

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
	err = page.Navigate("https://manage.baotong-soft.com/")
	if err != nil {
		log.Fatal("无法访问网页:", err)
	}

	// 使用 CSS 选择器查找用户名和密码输入框
	usernameInput := page.FindByID("username")
	passwordInput := page.FindByID("password")

	// 输入登录信息
	err = usernameInput.Fill("your_username")
	if err != nil {
		log.Fatal("无法填写用户名:", err)
	}

	err = passwordInput.Fill("your_password")
	if err != nil {
		log.Fatal("无法填写密码:", err)
	}

	// 提交登录表单（可选）
	err = passwordInput.Submit()
	if err != nil {
		log.Fatal("无法提交表单:", err)
	}

	// 关闭 WebDriver 会话
	err = driver.Stop()
	if err != nil {
		log.Fatal("无法停止 WebDriver:", err)
	}
}

```


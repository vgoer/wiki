---
title: 3.robotgo
description: 
published: 1
date: 2023-09-05T04:39:55.355Z
tags: 
editor: markdown
dateCreated: 2023-09-02T03:24:50.415Z
---

<center>robotgo</center>







[toc]



## rogotgo

> Golang 跨平台自动化系统，控制键盘、鼠标、位图、图像、读取屏幕，进程、窗口句柄以及全局事件监听
>
> 项目地址： [robotgo](https://github.com/go-vgo/robotgo)



### 下载

```go
1. 下载库
go get github.com/go-vgo/robotgo

2. 初始化模块
go mod init m(名称)

3.添加需要用到但go.mod中查不到的模块,删除未使用的模块
go mod tidy  
```

> eg:我下载报错了，说没有gcc `cgo: exec gcc: exec: "gcc": executable file not found in %PATH%`
>
> 解决:[简书毛毛](https://www.jianshu.com/p/c38483c30fb7)
>
> 博客:[简书](https://www.jianshu.com/p/f55680240923),gitee:[titee](https://gitee.com/veni0/robotgo#https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Fgo-vgo%2Frobotgo%2Fblob%2Fmaster%2Fdocs%2Fdoc_zh.md)

### 1. go鼠标

```go
package main

import (
	"github.com/go-vgo/robotgo"
)

func main() {

	// 获取当前鼠标所在的位置
	x, y := robotgo.GetMousePos()
	println(`x：`, x, ` y：`, y)
    
    
/* ================= 按键操作 == */

    //滚轮滚动 ScrollMouse
    // 向上滚动：3行
    robotgo.ScrollMouse(3, `up`)
    // 向下滚动：2行
    robotgo.ScrollMouse(2, `down`)
    
    
        // 按下鼠标左键
    // 第1个参数：left(左键) / center(中键，即：滚轮) / right(右键)
    // 第2个参数：是否双击
    robotgo.MouseClick(`right`, false)
	//双击左键
	robotgo.MouseClick(`left`, true)

    // 按住鼠标左键
    robotgo.MouseToggle(`down`, `left`)
    // 解除按住鼠标左键
    robotgo.MouseToggle(`up`, `left`)
    
    /* ========================= 位置操作 ======================== */

    // 将鼠标移动到屏幕 x:800 y:400 的位置（闪现到指定位置）
        robotgo.MoveMouse(800, 400)

    // 将鼠标移动到屏幕 x:800 y:400 的位置（模仿人类操作）
    robotgo.MoveMouseSmooth(800, 400)

    // 将鼠标移动到屏幕 x:800 y:400 的位置（模仿人类操作）
    // 第3个参数：纵坐标x 的延迟到达时间
    // 第4个参数：横坐标y 的延迟到达时间
    robotgo.MoveMouseSmooth(800, 400, 20.0, 200.0)
    
    
    /* ========================= 组合操作 ======================== */

    // 移动鼠标到 x:800 y:400 后，双击鼠标左键
    robotgo.MoveClick(800, 400, `left`, true)
    
}

```



### 2.键盘

```go
func main(){
    /* ========================= 键盘按下 ======================== */

    // 模拟按下1个键：打开开始菜单（win）
    robotgo.KeyTap(`command`)
    // 模拟按下2个键：打开资源管理器（win + e）
    robotgo.KeyTap(`e`, `command`)
    // 模拟按下3个键：打开任务管理器（Ctrl + Shift + ESC）
	robotgo.KeyTap(`esc`, `shift`, `control`)
    
    
    /* ========================= 按住不放 ======================== */

    // 一直按住 A键不放
    robotgo.KeyToggle(`a`, `down`)
    // 解除按住 A键
    robotgo.KeyToggle(`a`, `up`)
    
}
```

| A-Z、a-z    | 字母键              |
| ----------- | ------------------- |
| 0-9         | 主键盘上 的 数字键  |
| num0 - num9 | 小键盘上 的 数字键  |
| f1 - f24    | 主键盘上方的 功能键 |

| backspace | 删除 |
| --------- | ---- |
| delete    | 删除 |
| enter     | 确认 |
| tab       | tab  |
| esc       |      |
| escape    |      |

| cmd      | win  |
| -------- | ---- |
| up       | ↑键  |
| down     | ↓键  |
| right    | →键  |
| left     | ←键  |
| home     |      |
| end      |      |
| pageup   |      |
| pagedown |      |

> 更多参考[简书](https://www.jianshu.com/p/5f508bcb24b3)

```go
// 键盘拓展

func main(){
    
    // 键盘写入
	robotgo.TypeStr("hello go")
    // 等待1秒
    robotgo.Sleep(1)
    
}
```



### 3.屏幕



```go
// 屏幕

func main(){
    
    //获取鼠标位置
    x, y := robotgo.GetMousePos()
    fmt.Println("pos: ", x, y)
    
    // 获取屏幕上一个像素的颜色（十六进制颜色，不带 #）
    // 第1、2个参数：像素的座标
    color := robotgo.GetPixelColor(800, 400)
    println(`颜色：`, color)
    
     // 获取屏幕大小
    width, height := robotgo.GetScreenSize()
    println(`宽：`, width, ` 高：`, height)
    
     // 屏幕截图
    // 第1、2个参数：截图的座标
    // 第3、4个参数：截图的宽高
    bitmap := robotgo.CaptureScreen(0, 0, 800, 400)
    // 
    全屏截图
    // bitmap := robotgo.CaptureScreen(0, 0, width, height)

    // 保存截图
    // 第1个参数：截图数据
    // 第2个参数：保存的文件名，不支持UTF-8字符的文件名字（例如：中文）
    robotgo.SaveBitmap(bitmap, `screen.png`)
}
```









### 自动化

> 类似按键精灵

```go
package main

import (
	"fmt"
	"time"

	"github.com/go-vgo/robotgo"
)

const (
	AppIconPath  = "./img/appicon.png"
	SendWinPath  = "./img/me.png"
	SendBtnPath  = "./img/but.png"
	IconFilePath = "./img/appicon.png"
)

type Trip struct {
	Icon []int
}

func main() {
	trip := Trip{
		Icon: make([]int, 4),
	}

	for {
		var a int
		fmt.Println("请选择操作 1 获取图标截图 2 点击图标 3 锁定聊天窗口并发送内容")
		fmt.Scanln(&a)

		switch a {
		case 1:
			trip.getScreen()
			trip.getShot()
		case 2:
			trip.clickIcon()
		case 3:
			trip.clickMessage()
		}
	}
}

func (t *Trip) getScreen() {
	fmt.Println("请在要打开的程序图标左上角点击左键")
	ok := robotgo.AddEvent("mleft")

	if ok {
		t.Icon[0], t.Icon[1] = robotgo.GetMousePos()
		fmt.Println("---请在要截图右下角下左键---", t.Icon)
	}

	for i := 0; i < 5; i++ {
		ok = robotgo.AddEvent("mleft")

		if ok {
			x, y := robotgo.GetMousePos()
			t.Icon[2] = x - t.Icon[0]
			t.Icon[3] = y - t.Icon[1]
			fmt.Println("图片位置:", t.Icon)
		}
	}
}

func (t *Trip) getShot() {
	fmt.Println("正在获取截图....")
	bitMap := robotgo.CaptureScreen(t.Icon...)
	robotgo.SaveBitmap(bitMap, AppIconPath)
	fmt.Println("已保存截图")
}

func (t *Trip) clickIcon() {
	x, y := t.getImgXY(IconFilePath)

	if x < 0 || y < 0 {
		fmt.Println("获取程序坐标失败")
		return
	}

	t.clickShot(x, y)
	t.clickMessage()
}

func (t *Trip) clickShot(x, y int) {
	robotgo.MouseToggle("up")
	robotgo.MoveClick(x, y, "left", true)
}

func (t *Trip) getImgXY(img string) (x, y int) {
	bitMap := robotgo.OpenBitmap(img)
	fx, fy := robotgo.FindBitmap(bitMap)
	fmt.Println(fx, "--", fy)

	if fx < 0 && fy < 0 {
		fmt.Println("获取坐标失败，重试获取")
		time.Sleep(time.Second * 2) // 延迟2秒等待图像资源准备
		return t.getImgXY(img)
	} else {
		return fx, fy
	}
}

func (t *Trip) clickMessage() {
	x, y := t.getImgXY(SendWinPath)

	if x < 0 || y < 0 {
		fmt.Println("获取程序坐标失败")
		return
	}

	t.clickShot(x, y)

	msgs := []string{"golang", "python"}

	for i := 0; i < len(msgs); i++ {
		t.sendMessage(msgs[i])
	}
	// t.sendMessage("Hello World!")

}

func (t *Trip) sendMessage(msg string) {
	time.Sleep(time.Second)
	robotgo.TypeString(msg)
	time.Sleep(time.Second)
	robotgo.KeyTap("enter")
	time.Sleep(time.Second)
	robotgo.KeyTap("lctrl", "enter")
	t.sendOut()
}

func (t *Trip) sendOut() {
	x, y := t.getImgXY(SendBtnPath)

	if x < 0 || y < 0 {
		fmt.Println("获取程序坐标失败")
		return
	}

	t.clickShot(x, y)
}

```














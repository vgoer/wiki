---
title: 5.cli Color
category: Golang
time: 2022-01-10
tag:
  - golang

footer: 人面不知何处去，桃花依旧笑春风。——崔护《题都城南庄》
---
<center>颜色命令行</center>



[toc]





### CLI Color

> 跨平台的golang终端颜色库
>
> github:[gookit/color](https://github.com/gookit/color)



### 1. 开始

> 安装

```go
// 初始化包
go mod init color 
// 安装
go get github.com/gookit/color
```

> 列子：

``` go
package main

import (
	"fmt"
	
	"github.com/gookit/color"
)

func main() {
	// 简单快速的使用，跟 fmt.Print* 类似
	color.Redp("Simple to use color")
	color.Redln("Simple to use color")
	color.Greenp("Simple to use color\n")
	color.Cyanln("Simple to use color")
	color.Yellowln("Simple to use color")

	// 简单快速的使用，跟 fmt.Print* 类似
	color.Red.Println("Simple to use color")
	color.Green.Print("Simple to use color\n")
	color.Cyan.Printf("Simple to use %s\n", "color")
	color.Yellow.Printf("Simple to use %s\n", "color")

	// use like func
	red := color.FgRed.Render
	green := color.FgGreen.Render
	fmt.Printf("%s line %s library\n", red("Command"), green("color"))

	// 自定义颜色
	color.New(color.FgWhite, color.BgBlack).Println("custom color style")

	// 也可以:
	color.Style{color.FgCyan, color.OpBold}.Println("custom color style")
	
	// internal style:
	color.Info.Println("message")
	color.Warn.Println("message")
	color.Error.Println("message")
	
	// 使用内置颜色标签
	color.Print("<suc>he</><comment>llo</>, <cyan>wel</><red>come</>\n")
	// 自定义标签: 支持使用16色彩名称，256色彩值，rgb色彩值以及hex色彩值
	color.Println("<fg=11aa23>he</><bg=120,35,156>llo</>, <fg=167;bg=232>wel</><fg=red>come</>")

	// apply a style tag
	color.Tag("info").Println("info style text")

	// prompt message
	color.Info.Prompt("prompt style message")
	color.Warn.Prompt("prompt style message")

	// tips message
	color.Info.Tips("tips style message")
	color.Warn.Tips("tips style message")
}
```



### 2.基础颜色

> 提供通用的API方法：`Print` `Printf` `Println` `Sprint` `Sprintf`

```go
color.Bold.Println("bold message")
color.Black.Println("bold message")
color.White.Println("bold message")
color.Gray.Println("bold message")
color.Red.Println("yellow message")
color.Blue.Println("yellow message")
color.Cyan.Println("yellow message")
color.Yellow.Println("yellow message")
color.Magenta.Println("yellow message")

// 只修改前景色
color.FgCyan.Printf("Simple to use %s\n", "color")
// 背景色 
color.BgRed.Printf("Simple to use %s\n", "color")
```



> 构建风格：  color.Style{color.FgCyan, color.BgBlue, color.OpBold}.Printf("color is bettf\n")

```go
// 仅设置前景色
color.FgCyan.Printf("Simple to use %s\n", "color")
// 仅设置背景色
color.BgRed.Printf("Simple to use %s\n", "color")

// 完全自定义: 前景色 背景色 选项
// 完全自定义: 前景色 背景色 选项
style := color.New(color.FgGreen, color.BgRed, color.OpBold)
style.Println("custom color style")

// 也可以:
color.Style{color.FgCyan, color.BgBlue, color.OpBold}.Printf("color is bettf\n")
```

> 直接设置控制台属性

```go
// 设置console颜色
color.Set(color.FgCyan)

// 输出信息
fmt.Print("message")

// 重置console颜色
color.Reset()
```



### 2. 扩展风格

> 扩展

```go
// print message
color.Info.Println("Info message")
color.Note.Println("Note message")
color.Notice.Println("Notice message")
color.Error.Println("Error message")
color.Danger.Println("Danger message")
color.Warn.Println("Warn message")
color.Debug.Println("Debug message")
color.Primary.Println("Primary message")
color.Question.Println("Question message")
color.Secondary.Println("Secondary message")
```

> 简约提示风格

```go
// 提示
color.Info.Tips("Info tips message")
color.Note.Tips("Note tips message")
color.Notice.Tips("Notice tips message")
color.Error.Tips("Error tips message")
color.Danger.Tips("Danger tips message")
color.Warn.Tips("Warn tips message")
color.Debug.Tips("Debug tips message")
color.Primary.Tips("Primary tips message")
color.Question.Tips("Question tips message")
color.Secondary.Tips("Secondary tips message")
```

> 着重提示风格

```go
color.Info.Prompt("Info prompt message")
color.Note.Prompt("Note prompt message")
color.Notice.Prompt("Notice prompt message")
color.Error.Prompt("Error prompt message")
color.Danger.Prompt("Danger prompt message")
```

> 强调着重风格

```go
// 加粗了，换行
color.Warn.Block("Warn block message")
color.Debug.Block("Debug block message")
color.Primary.Block("Primary block message")
color.Question.Block("Question block message")
color.Secondary.Block("Secondary block message")
```



### 3. 256色彩

> 256色彩在 `v1.2.4` 后支持Windows CMD,PowerShell 环境
>
> - `color.C256(val uint8, isBg ...bool) Color256`

```go
c := color.C256(100) // fg color
c.Println("message")

c2 := color.C256(200, true) // true bg color
c2.Println("200颜色")

// 同时设置前景色和背景色
c3 := color.S256(32, 203)
c3.Println("前景色和背景色")

// 加粗和设置前后色
s := color.S256(32, 203)
s.SetOpts(color.Opts{color.OpBold})
```

![color-tags](https://github.com/gookit/color/raw/master/_examples/images/color-256.png)

### 5.RGB/True色彩使用

> RGB色彩在 `v1.2.4` 后支持 Windows `CMD`, `PowerShell` 环境

```go
// reb
color.RGB(30, 144, 255).Println("message. use RGB number")
// 六十进制
color.HEX("#1976D2").Println("blue-darken")
color.HEX("#D50000", true).Println("red-accent. use HEX style")

color.RGBStyleFromString("213,0,0").Println("red-accent. use RGB number")
color.HEXStyle("eee", "D50000").Println("deep-purple color")
```

> 前景色和后景色

```go
1. RGB
c := color.RGB(30,144,255) // fg color 
c := color.RGB(30,144,255, true) // bg color

2. HEX
c := color.HEX("ccc") // 也可以写为: "cccccc" "#cccccc" fg
c = color.HEX("aabbcc", true) // as bg color
```

> RGB风格设置

```
// 同时设置前景色和后景色
1. RGB
s := color.NewRGBStyle(RGB(20, 144, 234), RGB(234, 78, 23))
2. HEX
s := color.HEXStyle("11aa23", "eee")

// 设置加粗
s.SetOpts(color.Opts{color.OpBold})
```



![color-rgb](https://github.com/gookit/color/raw/master/_examples/images/color-rgb.png)



### 5.颜色标签

> 内置的颜色标签，可以非常方便简单的构建自己需要的任何格式
>
> > 同时支持自定义颜色属性: 支持使用16色彩名称，256色彩值，rgb色彩值以及hex色彩值

```go
// 内置颜色标签
// 使用内置的 color tag
color.Print("<suc>he</><comment>llo</>, <cyan>wel</><red>come</>")
color.Println("<suc>hello</>")
color.Println("<error>hello</>")
color.Println("<warning>hello</>")

// 自定义颜色属性
color.Print("<fg=yellow;bg=black;op=underscore;>hello, welcome</>\n")

// 自定义颜色属性: 支持使用16色彩名称，256色彩值，rgb色彩值以及hex色彩值
color.Println("<fg=11aa23>he</><bg=120,35,156>llo</>, <fg=167;bg=232>wel</><fg=red>come</>")
```

> color.Tag 给定的颜色风格标签

```go
color.Tag("info").Print("info style text")

```







> 命令行生成字符 [字符](http://patorjk.com/software/taag/#p=display&f=Slant&t=Composer)

```go
// golang
      .         
 _  _ | _.._  _ 
(_](_)|(_][ )(_]
._|          ._|

```

> go 等待时间执行

```go
// 等待两秒
time.Sleep(time.Second * 2) 
os.Exit(0)
```



> 结合上面的写的小栗子

```go
package main

import (
	"bufio"
	"os"
	"time"

	"github.com/gookit/color"
)
func main() {

	var str string = `
		o                                                                          o                o     
		_<|>_                                                                      _<|>_             <|>    
																									< >    
		o       o__ __o/   o      o     o__ __o/      __o__    __o__  \o__ __o     o    \o_ __o     |     
		<|>     /v     |   <|>    <|>   /v     |      />  \    />  \    |     |>   <|>    |    v\    o__/_ 
		/ \    />     / \  < >    < >  />     / \     \o     o/        / \   < >   / \   / \    <\   |     
		\o/    \      \o/   \o    o/   \      \o/      v\   <|         \o/         \o/   \o/     /   |     
		|      o      |     v\  /v     o      |        <\   \\         |           |     |     o    o     
		< >     <\__  / \     <\/>      <\__  / \  _\o__</    _\o__</  / \         / \   / \ __/>    <\__  
		|                                                                               \o/               
	o__     o                                                                                |                
	<\__ __/>                                                                               / \               
			
	`

	style := color.S256(219)

	style.Println(str)

	color.Info.Tips("猜猜这个是什么?")
	scanner := bufio.NewScanner(os.Stdin)

	for scanner.Scan() {
		var text = scanner.Text()
		if text == "javascript" {
			color.Secondary.Println("虽然猜对了,也不能阻止你是个傻b的事实")
			// 程序等待两秒
			time.Sleep(time.Second * 2)
			os.Exit(0)
		} else {
			color.Error.Prompt("傻b,这都能猜错")
		}
	}
}

```


---
title: 1.excel
category: Golang
time: 2022-01-10
tag:
  - golang

footer: 人面不知何处去，桃花依旧笑春风。——崔护《题都城南庄》
---
<center>表格</center>

[toc]



## go 表格

> go 操作excel表格 
>
> 借鉴:[平也](https://juejin.cn/post/6882343145661874190#heading-0)



### 1. 关键术语

> 了解 excel 中的几个关键术语，如下图所示，
>
> ①为sheet，也就是表格中的页签；
>
> ②为row，代表 excel 中的一行；
>
> ③为cell，代表 excel 中的一个单元格。



![img](http://pingyeaa.oss-cn-shenzhen.aliyuncs.com/image-1602348679727.png)



### 开源库

> 创建表格前需要先引入 excel 库，我们以比较热门的 **tealeg/xlsx** 库为例。
>
> 地址：[xlsx](https://github.com/tealeg/xlsx)



#### 1. 创建

1. 下载库

```go
go get github.com/tealeg/xlsx
```

2. 创建文件，拿到文件句柄

```go
file := xlsx.NewFile()
```

3. 创建一个 `成绩` 的 sheet

```go
sheet, err := file.AddSheet("成绩表")
if err != nil {
    panic(err.Error())
}
```

4. 创建第一行，作为表头

```go
row := sheet.AddRow()
```

5. 创建一个单元格 

```go
cell := row.AddCell() 
```

6. 设置单元格内容

```
cell.Value = "姓名"
```

```go
// 第二个单元格也一样  但是 注意是  = 不是 :=
cell = row.AddCell()
cell.Value = "性别"
```

7. 第二行，我们的内容

```go
// 也和第一行一样
row = sheet.AddRow()
cell = row.AddCell()
cell.Value = "张三"
cell = row.AddCell()
cell.Value = "男"
```

8. 保存文件

```go
err = file.Save("godome.xlsx")
if err != nil{
    panic(err.Error())
}
```

>  你就会在当前目录下看到惊喜了

> eg:创建两个sheet,学生表和成绩表，存入一些数据

```go
// eg
package main

import (
	"strconv"

	"github.com/tealeg/xlsx"
)

func main() {
	student()
	addHome()
}

// 学生表
func student() {
	stuMap := make(map[string]int, 10)
	stuMap["小红"] = 100
	stuMap["小白"] = 100
	stuMap["小青"] = 1
	stuMap["小蓝"] = 11
	stuMap["小爱"] = 100

	// 创建文件
	file := xlsx.NewFile()
	// 创建一个sheet
	sheet, err := file.AddSheet("学生表")
	if err != nil {
		panic(err.Error())
	}
	// 创建一行 一单元格 填充一个value
	row := sheet.AddRow()
	cell := row.AddCell()
	cell.Value = "姓名"
	//第二个单元格
	cell = row.AddCell()
	cell.Value = "性别"
	for k, v := range stuMap {
		row := sheet.AddRow()
		cell := row.AddCell()
		cell.Value = k
		//第二个单元格
		cell = row.AddCell()
		// int转换为字符串
		strV := strconv.Itoa(v)
		cell.Value = strV

	}

	err = file.Save("stu.xlsx")
	if err != nil {
		panic(err.Error())
	}

}

func addHome() {
	stuHome := [...]string{"北京", "上海", "天津", "南京", "云南"}

	// 创建文件
	file := xlsx.NewFile()
	// 创建一个sheet
	sheet, err := file.AddSheet("住址")
	if err != nil {
		panic(err.Error())
	}

	row := sheet.AddRow()
	cell := row.AddCell()
	cell.Value = "序号"
	cell = row.AddCell()
	cell.Value = "家庭住址"
	for i := 0; i < len(stuHome); i++ {
		row := sheet.AddRow()
		cell := row.AddCell()
		// int转换为字符串
		strV := strconv.Itoa(i)
		cell.Value = strV
		cell = row.AddCell()
		cell.Value = stuHome[i]
	}

	err = file.Save("home.csv")
	if err != nil {
		panic(err.Error())
	}

}
```

> 卧槽，go 一个 go build 编译到任何操作系统也太牛了吧 哈哈哈哈



#### 2.读取表格

> 读取表格，修改表格

```go
output, err := xlsx.FileToSlice("stu.xlsx") //切片
if err != nil {
  panic(err.Error())
}
```

> 打印

```go
log.Println(output)  //切片
[[[姓名 性别] [小青 1] [小爱 100] [小红 100] [小蓝 11] [小白 100]]]
```

```go
// 读取表格
func aditXls() {
	output, err := xlsx.FileToSlice("stu.xlsx")
	if err != nil {
		panic(err.Error())
	}
	for rowIndex, row := range output[0] {
		for cellIndex, cell := range row {
			log.Println(fmt.Sprintf("第%d行,第%d个单元格:%s", rowIndex, cellIndex, cell))
		}
	}
}
```





#### 3.修改表格

> 只是读取表格内容可能在特定场景下无法满足需求，
>
> 有时候需要对表格内容进行更改。

1. 打开文件

```go
file, err := xlsx.OpenFile("stu.xlsx")
if err != nil {
  panic(err.Error())
}
```

2. 直接修改

```go
file.Sheets[0].Rows[1].Cells[0].Value = "小草"
```

3. 保存

```go
err = file.Save("stu_edit.xlsx")
if err != nil {
  panic(err.Error())
}
```



#### 4.修改样式

> 开源库不仅支持内容的编辑，还支持表格的样式设置
>
> 样式统一由结构体 Style 来负责。

```go
type Style struct {
	Border          Border
	Fill            Fill
	Font            Font
	ApplyBorder     bool
	ApplyFill       bool
	ApplyFont       bool
	ApplyAlignment  bool
	Alignment       Alignment
	NamedStyleIndex *int
}
```

>  设置居中

1. 首先要实例化样式对象。

```go
style := xlsx.NewStyle()
```

2. 居中属性

```go
style.Alignment = xlsx.Alignment{
  Horizontal:   "center",
  Vertical:     "center",
}
```

3. 选择要设置的单元格

```go
file.Sheets[0].Rows[0].Cells[0].SetStyle(style)
```

其他样式 很多呀

```go
style.Font.Color = xlsx.RGB_Dark_Red
style.Fill.BgColor = xlsx.RGB_Dark_Green
```
























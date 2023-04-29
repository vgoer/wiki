---
title: 02.template库
description: template库
published: 1
date: 2023-04-29T17:42:53.232Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T10:19:25.041Z
---

<center>template</center>



[toc]



## template

> 定义了数据驱动的==文本输出==



```go
// 解析变量
func temp1() {

	name := "goer"
	tmpstr := "hello {{.}}" // 只有一个当前变量
	// 生成新模板
	t := template.New("test")
    // 解析
	t2, err := t.Parse(tmpstr)
	if err != nil {
		log.Fatal(err)
	}
	// 执行
	t2.Execute(os.Stdout, name)
}
```

```go
// 结构体
func temp2() {
	per := Person{
		Name: "tom",
		Age:  10,
	}
	str := "name : {{.Name}}, age: {{.Age}}"

	t, err := template.New("test").Parse(str)
	if err != nil {
		log.Fatal(err)
	}
	t.Execute(os.Stdout, per)
}
```



### 1.html文件

> 一般模板是html文件

```go
// 定义 html文件
<body>
   // 获取当前变量
    <h2>{{.}}</h2>
</body>
```

```go

func tmpHtml(w http.ResponseWriter, r *http.Request) {
	// 解析html文件
	t, err := template.ParseFiles("index.html")
	if err != nil {
		panic(err)
	}
	t.Execute(w, "hello world")
}

func tmpServer() {
	server := http.Server{
		Addr: "localhost:9999",
	}
	// html 模板函数文件
	http.HandleFunc("/hello", tmpHtml)
	server.ListenAndServe()
}
```



### 2. 文本和空格

> 对解析的内容不要随意缩进和换行

```go
{{23 -}} < {{- 40}} => 23<40
```

> 去掉后面空格`-}}`  去掉前面空格`{{- `



### 3. 注释

```go
{{-/* aaaaaa */-}}
```



### 4. 管道

```go

```





### 5. 变量

```go
// 赋值 打印 长度
{{$name := "tom"}}
{{$name}}
{{len $name}}
```



### 6. 条件

```go
{{$name := true}}
{{$name}}
{{if $name}} T1 {{else}} T0 {{end}}
```

### 7.运算符

```go
// 和linux shell 运算符一样
eq  等于
ne  不等于
lt  < 小于
le  小于等于
gt  > 大于
ge  大于等于
```

```html
{{$age := 20}}

{{if (ge $age 10)}}
<h3>成年了</h3>
{{else}}
<h3>未成年</h3>
{{end}}
```

### 8.循环

```go
func tmpHtml(w http.ResponseWriter, r *http.Request) {
	// 解析html文件
	t, err := template.ParseFiles("index.html")
	if err != nil {
		panic(err)
	}
    s := []string{"go","php","javascript","java","c","c++"}
	t.Execute(w,c)
}

// html
{{range $x := . -}}
{{println $x}}
{{- end}}
```



### 9.模板

> 公共部分

```html
// header.html
    {{define "header"}}
    <p>header 部分...</p>
    {{end}}
// footer.html
    {{define "footer"}}
    <p>footer 部分...</p>
    {{end}}
// index.html
    {{template "header"}}
    <h1>首页</h1>
    {{template "footer"}}
```

```go
//访问
func tmpHtml(w http.ResponseWriter, r *http.Request) {
	// 解析html文件 
	t, err := template.ParseFiles("index.html", "header.html", "footer.html")
	if err != nil {
		panic(err)
	}
	t.Execute(w, nil)
}

func tmpServer() {
	server := http.Server{
		Addr: "localhost:9999",
	}
	// html 模板函数文件
	http.HandleFunc("/index", tmpHtml)
	server.ListenAndServe()
}
```


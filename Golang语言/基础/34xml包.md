---
title: 34.xml包
description: xml包
published: 1
date: 2023-06-09T10:12:28.670Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T09:59:20.815Z
---

<center>xml包</center>



[toc]



## xml包

> 实现xml解析  和json类似

```go
// xml
type Person struct {
	XMLName xml.Name `xml:"Person"`
	Name    string   `xml:"name"`
	Age     int      `xml:"age"`
	Love    string   `xml:love`
}

func main() {

	per := Person{
		Name: "goer",
		Age:  10,
		Love: "萝卜",
	}
	// b, _ := xml.Marshal(per)
	// 格式化字符  转化结构体  前缀   分割符
	b, _ := xml.MarshalIndent(per, " ", "  ")
	fmt.Printf("%v\n", string(b))
}
```

```go 
// xml转对象
s := `
	<Person>
	<name>goer</name>
	<age>10</age>
	<Love>萝卜</Love>
  	</Person>
	`
b := []byte(s)
var p Person

xml.Unmarshal(b, &p)
fmt.Printf("p: %v\n", p)
```

```go
// 读取文件xml
// io 读取的就是切片
b, _ := ioutil.ReadFile("a.xml")
var p Person

xml.Unmarshal(b, &p)
fmt.Printf("p: %v\n", p) //{{ Person} goer 10 萝卜}
```

```go
// 写xml
per := Person{
    Name: "goer",
    Age:  10,
    Love: "萝卜",
}

// 和json类似
f, _ := os.OpenFile("b.xml", os.O_RDONLY|os.O_CREATE, 0777)
defer f.Close()
e := xml.NewEncoder(f)
e.Encode(per)
```












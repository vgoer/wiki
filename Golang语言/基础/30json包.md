---
title: 30.json包
description: json包
published: 1
date: 2023-04-24T10:31:00.261Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T09:58:20.846Z
---

<center>json包</center>



[toc]





## json包

> 实现json的编码和解码

```go
// Marshal 结构体装换对象
// 大写才能转化 是什么问题呢
type Dog struct {
	Name string
	Age  int
	Love string
}

func main() {

	dog := Dog{Name: "goer", Age: 2, Love: "骨头"}

	// 返回字符切片
	b, _ := json.Marshal(dog)
	fmt.Printf("b: %v\n", string(b))  // {"Name":"goer","Age":2,"Love":"骨头"}
}
```

```go
// Unmarshal()
sli := []byte(`{"Name":"goer","Age":2,"Love":"骨头"}`)

// 声明 dog 和类型
var dog Dog
// 返回值是指针类型
json.Unmarshal(sli, &dog)
fmt.Printf("dog: %v\n", dog)
```

```go
// 嵌套类型
sli := []byte(`{"Name":"goer","Age":2,"Love":"骨头","Source":"[1,2,3,4]"}`)

    // map 类型 string 键 interface 任意类型值
    var f map[string]interface{}
    json.Unmarshal(sli, &f)
    fmt.Printf("f: %v\n", f)
    for k, v := range f {
        fmt.Printf("%v:%v\n", k, v)
    }
}
```

```go
// 读json文件
f, _ := os.Open("a.json")
// NewDecoder 结构体
defer f.Close()
d := json.NewDecoder(f)
var v map[string]interface{}

d.Decode(&v)
fmt.Printf("v: %v\n", v)
```

```go
// 写入文件
// 打开文件只读
f, _ := os.OpenFile("a.json", os.O_RDONLY, 0777)

defer f.Close()
dog := Dog{
    Name:   "tom",
    Age:    20,
    Love:   "骨头",
    Source: []int{10, 20, 30},
}
// 解码 写入文件
e := json.NewEncoder(f)
// 对象写入文件
e.Encode(dog)
```




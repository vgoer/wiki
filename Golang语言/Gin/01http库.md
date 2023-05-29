---
title: 01.http库
description: http库
published: 1
date: 2023-05-29T04:11:36.933Z
tags: golang
editor: markdown
dateCreated: 2023-04-21T10:08:21.281Z
---

<center>http标准库</center>



[toc]



## Http标准库

> 发送网络请求 



### 1. get

```go
// 简单get
func geturl() {
	url := "https://www.baidu.com"

	// 发送get请求
	r, err := http.Get(url)
	if err != nil {
		log.Fatal(err)
	}
	// 关闭 请求体
	defer r.Body.Close()
	// 读取 到字节切片
	b, _ := ioutil.ReadAll(r.Body)
	fmt.Printf("b: %v\n", string(b))
}
```

```go
// 参数的get
func getPar() {
	params := url.Values{}

	Url, err := url.Parse("https://www.baidu.com")
	if err != nil {
		log.Fatal(err)
	}
	// 设置参数
	params.Set("key", "value")
	params.Set("city", "tianjin")
	// 如果参数有中文， 使用 URLEncode
	Url.RawQuery = params.Encode()
	urlPath := Url.String()
	fmt.Printf("urlPath: %v\n", urlPath)

	resp, err2 := http.Get(urlPath)
	if err2 != nil {
		log.Fatal(err2)
	}
	defer resp.Body.Close()

	b, _ := ioutil.ReadAll(resp.Body)
	fmt.Printf("b: %v\n", string(b))
}
```

```go
// json 转结构体
func getStr() {
	type Result struct {
		Args    string            `json:"args"`
		Headers map[string]string `json:"headers"`
		Origin  string            `json:"origin"`
		Url     string            `json:"url"`
	}

	res, err := http.Get("https://www.baidu.com")
	if err != nil {
		log.Fatal(err)
	}

	defer res.Body.Close()

	b, _ := ioutil.ReadAll(res.Body)
	fmt.Printf("b: %v\n", string(b))
	// json 转结构体
	var result Result
	_ = json.Unmarshal(b, &result)
	fmt.Printf("res: %#v\n", result)
}
```

```go
// 添加请求头信息
func addHeader() {
	// 新客户端
	client := &http.Client{}
	req, _ := http.NewRequest("GET", "https://www.baidu.com", nil)

	// 请求头
	req.Header.Add("user-agen", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.51 Safari/537.36")
	req.Header.Add("referer", "https://www.google.com")
	rep, _ := client.Do(req)
	b, _ := ioutil.ReadAll(rep.Body)
	fmt.Printf("b: %v\n", string(b))
}
```



### 2. post

> 更安全

```go
// postForm 
func testPost() {
	urlstr := "https://www.baidu.com"

	values := url.Values{}
	values.Set("key", "value")
	values.Set("add", "sub")

	// post 请求
	rep, err := http.PostForm(urlstr, values)
	if err != nil {
		log.Fatal(err)
	}

	defer rep.Body.Close()
	b, _ := ioutil.ReadAll(rep.Body)
	fmt.Printf("b: %v\n", string(b))
}
```



### 3. 服务端

> 创建一个服务端

```go
func testHttpServer() {
	// 请求函数
	f := func(resp http.ResponseWriter, req *http.Request) {
		io.WriteString(resp, "hello world")
	}
	// 响应路径 注意前面加  /
	http.HandleFunc("/hi", f)
	// 监听端口  注意前加 ：
	err := http.ListenAndServe(":9090", nil)
	if err != nil {
		log.Fatal(err)
	}
}
```

```go
1. go run 跑起来
2. 浏览器访问  http://localhost:9090/hi
```

```go
// 两个函数
func hello(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello\n")
}

func world(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "World\n")
}

func main() {
	server := http.Server{
		Addr: "127.0.0.1:8080",
	}
	// 第一个使用HandleFunc()为路径注册hello()函数handler
	// 第二个使用Handle()为路径注册转换后的handler
	http.HandleFunc("/hello", hello)
	http.Handle("/world", http.HandlerFunc(world))

	server.ListenAndServe()
}
```





> 实现并发处理

```go
```


---
title: 02axum
description: 
published: 1
date: 2023-08-30T10:37:12.242Z
tags: 
editor: markdown
dateCreated: 2023-08-30T10:37:07.247Z
---

<center>Axum</center>





[toc]







## Axum

> Axum是一个专注于人体工程学和模块化的Web应用程序框架。  [doc](https://docs.rs/axum/latest/axum/)
>
> tokio框架在rust异步当中相当流行。axum能很好地搭配tokio实现异步web 
>
> [开发之一](https://blog.csdn.net/qq_35270805/article/details/132364165)  [中间件 ](https://blog.csdn.net/qq_35270805/article/details/132410650) [blog](https://juejin.cn/user/677714513628055/columns)





### 1. hello 

```rust
use axum::{
    routing::get,
    Router,
};

#[tokio::main]
async fn main() {
    // 构建router
    let app = Router::new().route("/", get(|| async { "Hello, World!" }));

    // 运行hyper  http服务 localhost:3000
    axum::Server::bind(&"0.0.0.0:3000".parse().unwrap())
        .serve(app.into_make_service())
        .await
        .unwrap();
}

```

> 引入库

```rust
[dependencies]
axum = "0.6.19"
tokio = { version = "1.29.1", features = ["full"] }
tower = "0.4.13"
```

> 访问： `http://localhost:3000/` 我去，牛皮








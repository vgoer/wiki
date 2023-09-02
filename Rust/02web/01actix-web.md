<center>web启动</center>



[toc]





## web启动

> rust web 



### 1. 创建

```shell
# 1. 创建
cargo new rust_web && ce rust_web

# 2. 在我们的Cargo.toml文件中写入依赖：
[dependencies]
actix-web = "3"

# 3. 写入以下内容到src/main.rs，并使用cargo run运行项目：
```

```rust
use actix_web::{get, App, HttpResponse, HttpServer, Responder};

#[get("/")]
async fn hello() -> impl Responder {
    HttpResponse::Ok().body("Hello rust web!")
}

#[get("/list")]
async fn list() -> impl Responder {
    HttpResponse::Ok().body("list is blue")
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new()
            .service(hello)
            .service(list)
    })
    .bind("127.0.0.1:8080")?
    .run()
    .await
}
```

> 访问: `127.0.0.1:8080` wc， 看到了我们的项目
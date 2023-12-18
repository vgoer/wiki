---
title: 02Cargo
description: 
published: 1
date: 2023-07-26T13:02:31.158Z
tags: 
editor: markdown
dateCreated: 2023-07-26T13:02:29.704Z
---

<center>Cargo</center>



[toc]







## Cargo

> cargo: rust的构建系统和包管理工具 [cargo doc ](https://doc.rust-lang.org/cargo/) [cargo](https://crates.io/)





### 1. 安装

> 哈哈，别急，已经安装好了。
>
> 和==node->npm== 一样  ==rust->cargo==

```shell
# 查看
cargo --version

# 创建项目
cargo new hello_cargo(项目名称)

# Cargo.toml  信息和依赖
# src         源代码

# 构建项目
cargo build  # 生成 /target/debug/hello_cargo
# 构建版本 (正式发布)
cargo build --release # 优化，代码运行更快  /atrget/release/hello_cargo

# 构建和运行
cargo run   # 编译和运行

# 检查代码 确保编译通过
cargo check  # 常用的 调试
```

> rust 中 包和库成为 ==creat==

```shell
# 设置源
set RUSTUP_DIST_SERVER=https://mirrors.tuna.tsinghua.edu.cn/rustup
# 下载交叉编译的工具链
rustup target add x86_64-unknown-linux-gnu
# cargo 编译其他平台
rustc --print target-list  # 查看可编译的平台

cargo build --target <target-triple> # 指定平台

# win linux mac
cargo build --target x86_64-pc-windows-gnu  
cargo build --target x86_64-unknown-linux-gnu
cargo build --target x86_64-apple-darwin
```

> 交叉编译报错： [csdn](https://blog.csdn.net/qq_40307379/article/details/119753360)
>
> `yay -S mingw-w64`



### 2. 获取输入

> 小程序

```rust
// use导入 io 读取输入
use std::io;

fn main() {
    // 宏 只是打印
    println!("<<猜数字游戏>>");
    println!("猜测一个数");

    // rust 中所用变量是不可变的  immutable
    // 加入 mut 关键字就可以变了
    let foo = 1;
    // foo = 3;
    // println!("foo 为: {}",foo);

    let mut guess = String::new(); // 将一个可变的变量绑定到 空字符串上面

    io::stdin().read_line(&mut guess).expect("无法读取行");
    // io::Result 返回    ok , error   expect 执行错误

    // {} 占位符
    print!("你猜测的是:{},{}",guess,foo)

}
```





### 3. 随机数

> 随机数小程序

```shell
# 1.安装其他的 crates   	 https://crates.io/crates/rand
#命令
cargo add rand
# 文件  保存文件插件帮我们自己安装了
rand = "^0.8.5"
```

```rust
// 完整的才随机数的程序
// use导入 io 读取输入
use std::io;
// 导入随机数
use rand::Rng; // trait 其他语言的对象
// 枚举类型
use std::cmp::Ordering;

fn main() {
    println!("<<猜数字游戏>>");

    // 取一个 1 - 100的随机数
    let secret_num = rand::thread_rng().gen_range(1..101);
    println!("随机数字为:{}",secret_num);
    
    // 循环
    loop {
        println!("猜测一个数:");
        // i32 u32 i64
        let mut guess = String::new(); // 将一个可变的变量绑定到 空字符串上面


        io::stdin().read_line(&mut guess).expect("无法读取行"); 
        // io::Result 返回    ok , error   expect 执行错误

        // shadow 隐藏旧的变量   trim 去掉前后空白  parse 转整形
        let guess: u32 = match guess.trim().parse() {
            Ok(num)  => num,
            Err(_)   => continue
        };

        // {} 占位符
        print!("你猜测的是:{}",guess);

        // 字符串和整形比较  match 和 switch
        match guess.cmp(&secret_num) {
            // 小于
            Ordering::Less    => println!("太小了"),
            // 大于
            Ordering::Greater => println!("太大了"),
            // 等于
            Ordering::Equal   =>  {
                println!("猜对了");
                break;
            }
        }
    }

}
```










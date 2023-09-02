<center>Rust</center>



[toc]





## Rust

## 

> 一门赋予每个人构建可靠且高效软件的语言  [rust ](https://www.rust-lang.org/zh-CN) [github](https://github.com/rust-lang)
>
> 一门比较先进的语言： ==运行速度 内存安全  多处理器== 已经被应用到操作系统中





### 1. 应用

> 应用领域 firefox最开始研究

1. **传统命令行程序**  \- Rust 编译器可以直接生成目标可执行程序，不需要任何解释程序。
2. **Web 应用** - Rust 可以被编译成 WebAssembly

3. **网络服务器** - Rust 用极低的资源消耗做到安全高效
4. **嵌入式设备和操作系统** - Rust 同时具有JavaScript 一般的高效开发语法和 C 语言的执行效率



> 案例： 很多操作系统 (微软，谷歌，蚂蚁，linux，npm,等等)





### 2. 安装

> 下载安装： [rust](https://www.rust-lang.org/zh-CN/tools/install)

```shell
# linux & mac
curl -sSf https://sh.rustup.rs | sh

# 升级和卸载
rustup update  
rustup self uninstall

# 查看版本号
rustc --version

# rust 文档
rustup doc
```

> 编辑器： ==vscode + 插件rust-analyzer==





### 3. hello_world

> 每个语言学习的第一个程序

```rust
// hello_world.rs
fn main(){
    // rust macro(宏) !   没有！就是函数
    println!("hello world")
}
```

```shell
# 编译和运行
rustc hello_world.rs
./hello_world.exe 
```







​	
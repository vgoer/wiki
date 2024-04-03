<center>firework-rs烟花</center>





[toc]







## firework-rs烟花

> [github](https://github.com/Wayoung7/firework-rs)终端中的跨平台 ascii-art 烟花模拟器







## 1. 安装

> 详细使用看github

先安装rust

```shell
git clone https://github.com/Wayoung7/firework-rs.git
cd firework-rs
cargo run --release -- -d 0
```

> 或者全局安装

```shell
cargo install firework-rs
firework -d 0
```





### 命令示例

如果您已经安装了二进制文件：

启用渐变的无限烟花表演：

```
firework -g
```



启用循环和渐变的演示 1：

```
firework -l -g -d 1
```

> 该二进制文件现在有**5 个演示**，从**0**到**4**。

如果您尚未安装二进制文件：

首先`cd`进入项目根目录，然后运行：

```
cargo run --release -- -g
```



```
cargo run --release -- -l -g -d 1
```
---
title: tmux
description: tmux最全
published: 1
date: 2023-06-09T10:10:22.760Z
tags: app, tmux
editor: markdown
dateCreated: 2023-04-29T17:39:26.282Z
---

<center>tmux</center>





[toc]





## Tmux

> 终端复用神器 [tmux](https://www.ruanyifeng.com/blog/2019/10/tmux.html)





### 1.安装

> tmux

```shell
# Ubuntu 或 Debian
sudo apt-get install tmux

# CentOS 或 Fedora
sudo yum install tmux

# Mac
brew install tmux

#arch
sudo pacman -S tmux
```



### 2.会话

> `ctrl+d`  默认的前缀键
>
> `ctrl+d``+?` 帮助文档

```shell
# 新建会话
tmus new -s <session-name>

# 分离会话  ctrl+b d
tmux detach  

# 查看所有会话
tmux ls

# 接入会话
tmux attach -t <session-name>

# 杀死会话
tmux kill-session -t <session-name>

# 切换会话
tmus switch -t <session-name>
```

> 列出所有会话 `ctrl+b+s`



### 3. 窗格

> Tmux 可以将窗口分成多个窗格（pane）

```shell
Ctrl+b %：划分左右两个窗格。
Ctrl+b "：划分上下两个窗格。
Ctrl+b <arrow key>：光标切换到其他窗格。<arrow key>是指向要切换到的窗格的方向键，比如切换到下方窗格，就按方向键↓。
Ctrl+b ;：光标切换到上一个窗格。
Ctrl+b o：光标切换到下一个窗格。
Ctrl+b {：当前窗格与上一个窗格交换位置。
Ctrl+b }：当前窗格与下一个窗格交换位置。
Ctrl+b Ctrl+o：所有窗格向前移动一个位置，第一个窗格变成最后一个窗格。
Ctrl+b Alt+o：所有窗格向后移动一个位置，最后一个窗格变成第一个窗格。
Ctrl+b x：关闭当前窗格。
Ctrl+b !：将当前窗格拆分为一个独立窗口。
Ctrl+b z：当前窗格全屏显示，再使用一次会变回原来大小。
Ctrl+b Ctrl+<arrow key>：按箭头方向调整窗格大小。
Ctrl+b q：显示窗格编号。
```

### 4.窗格管理

> 新建窗格 `Ctrl+b c`
>
> 查看所有窗格 `Ctrl+b f`

```shell
Ctrl+b c：创建一个新窗口，状态栏会显示多个窗口的信息。
Ctrl+b p：切换到上一个窗口（按照状态栏上的顺序）。
Ctrl+b n：切换到下一个窗口。
Ctrl+b <number>：切换到指定编号的窗口，其中的<number>是状态栏上的窗口编号。
Ctrl+b w：从列表中选择窗口。
Ctrl+b ,：窗口重命名。
```



### 5.高阶功能

> 会话组功能： 可用于协调解决问题。比如服务器出问题了，两个人都可以登录到服务器，一个人可以看到另一个人的操作。[blog](https://www.roderickchan.cn/zh-cn/2023-02-10-socat%E5%8F%8D%E5%BC%B9shell-tmux%E5%85%B1%E4%BA%AB%E4%BC%9A%E8%AF%9D/#socat%E5%8F%8D%E5%BC%B9shell)

```shell
# 一个用户 新建一个会话组
tmux new -s groupSession

# 另一个用户连接该组
tmux new -t groupSession -s otherSession

# nb 可以同步通信了
```



### 6. 美化

> 美化和扩展 [git](https://github.com/gpakosz/.tmux)

```shell
git clone https://github.com/gpakosz/.tmux.git
# 建立软连接
ln -s -f .tmux/.tmux.conf
cp .tmux/.tmux.conf.local .

# 关闭终端，重新打开，如果不起作用，执行下面的命令
tmux source-file ~/.tmux.conf
```




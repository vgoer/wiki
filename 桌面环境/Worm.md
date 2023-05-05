---
title: Worm
description: worm桌面环境
published: 1
date: 2023-05-05T11:44:25.290Z
tags: desktop environment, worm
editor: markdown
dateCreated: 2023-04-19T15:31:24.022Z
---

<center>Worm</center>



[toc]





## Worm

> Worm 是一个用 Nim 语言编写的微型、动态、基于标记的窗口管理器。
>
> [worm](https://github.com/codic12/worm)



archinstall [error](https://www.reddit.com/r/archlinux/comments/w1pmlz/i_keep_getting_an_error_when_doing_archinstall/)

```shell
# error 
first delete the directory /etc/pacman.d/gnupg
rm -rf /etc/pacman.d/gnupg

# kill all gpg-agent process if running any
killall gpg-agent

# initialize the key database
pacman-key --init



# populate the archlinux keyring
pacman-key --populate archlinux
```



## 1. 安装

yay:[ins](https://www.tecmint.com/install-yay-aur-helper-in-arch-linux-and-manjaro/)

paru:[ins](https://ostechnix.com/how-to-install-paru-aur-helper-in-arch-linux/)

> 网络问题的话 虚拟机全局设置代理 [free](https://github.com/freefq/free)

```shell
# yay 助手

# warm-git
paru worm-git

# 配置文件
mkdir -p ~/.config/worm
# 复制配置文件
sudo cp /usr/share/doc/worm/examples/rc ~/.config/worm/rc
```
















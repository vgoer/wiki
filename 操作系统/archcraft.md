---
title: Archcraft OS
description: archcraft linux
published: 1
date: 2023-04-24T10:27:32.917Z
tags: linux
editor: markdown
dateCreated: 2023-04-18T17:34:17.449Z
---

<center>archcraft</center>





[toc]





## archcraft

> Archcraft 只是另一个 Linux 发行版，建立在 Arch Linux 之上。
>
> [官网](https://archcraft.io/index.html)





1. 中文字体  [up](https://zhuanlan.zhihu.com/p/511682089)

```go
sudo pacman -S adobe-source-han-sans-cn-fonts
```



2. 换源

```go
# https://zhuanlan.zhihu.com/p/102529503
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist-backup
sudo reflector --verbose -l 200 -p --sort rate --save /etc/pacman.d/mirrorlist
```



3. 安装key和更新

```go
sudo pacman -S archlinux-keyring
sudo pacman -Syyu
```



4. 更换内核

```go
sudo pacman -S linux-zen linux-zen-headers
```



5. 添加秘钥

```shell
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.sjtug.sjtu.edu.cn/archlinux-cn/$arch #上海交通大学源

#更新密钥
sudo pacman -S archlinuxcn-keyring
```



6. 输入法

```shell
sudo pacman -S fcitx-im #安装fcitx输入法框架，选择全部安装
sudo pacman -S fcitx-configtool #输入法配置文件
sudo pacman -S fcitx-rime #安装rime
sudo pacman -S fcitx-sogoupinyin # 安装sogoupinyin

#添加输入法配置文件
sudo vim ~/.xprofile

#新建一个配置文件后，添加以下内容，保存退出
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"

# 四叶草输入法
https://www.fkxxyz.com/d/cloverpinyin/

1.  安装 fcitx 
sudo pacman -S fcitx fcitx-qt5 fcitx-configtool
yay -S rime-cloverpinyin
```



7. 软件 [up](https://linux265.com/news/3544.html)

```shell
sudo pacman -S google-chrome  
sudo pacman -S netease-cloud-music
sudo pacman -S git
# oh my szsh
sudo pacman -S zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" # 下载并配置ohmyzsh
chsh -s /bin/zsh  #更换默认bash，重启后生效
sudo pacman -S visual-studio-code
sudo pacman -S typora
sudo pacman -S neofetch
```



8. wps

```go
sudo pacman -S wps-office     # 安装wps
sudo pacman -S ttf-wps-fonts  # 安装wps字体
// 配置
$ sudo vim /usr/bin/wps     # 编辑wps配置文件
# 在 紧跟#!/bin/bash后添加下列三行
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```



9. 科学上网

```go
sudo pacman -S shadowsocks-qt5
sudo pacman -S proxychains-ng
```



10. idea

```shell
# je 下载
# 淘宝购买激活码
```

11.其他好用的软件

```shell
btop 
sudo pacman -S screenkey
sudo pacman -S gparted 
sudo pacman -S flameshot  
yay -S picgo cava  lolcat
```

12. vscode设置字体

```shell
# https://github.com/tonsky/FiraCode
sudo pacman -S ttf-fira-code
# 打开vscode设置字体
'Fira Code',
```



### 重要

> 安装其他桌面环境 [os](https://archcraft.io/gallery.html)

```shell
# i3wm 
sudo pacman -Syyu && sudo pacman -S archcraft-i3wm

# dwm 
sudo pacman -Syy && sudo pacman -S archcraft-dwm

#berry
sudo pacman -S archcraft-berry

# herbstluftwm
sudo pacman -Sy archcraft-herbstluftwm
#xmonad
sudo pacman -Syyu && sudo pacman -S archcraft-xmonad
```














---
title: DWM
description: dwm桌面环境
published: 1
date: 2023-06-09T10:10:55.020Z
tags: desktop environment
editor: markdown
dateCreated: 2023-04-18T17:27:02.225Z
---

<center>dwm</center>





[toc]



## DWM

> DWM:[dwm](https://dwm.suckless.org/) 动态窗口管理器



### 1. 下载

> 下载安装 [up](https://yaocc.cc/2022/07/10/dwm/)

```shell
git clone https://git.suckless.org/dwm


安装xserver环境(如果你未安装图形化环境的话)
sudo pacman -S xorg-server
sudo pacman -S xorg-apps
sudo pacman -S xorg-xinit
#字体
sudo pacman -S noto-fonts-cjk

    #安装一个终端
    sudo pacman -S alacritty
    # 修改启动终端 
    static const char *termcme[] = { "alacritty",NULL}

sudo make clean install  //安装

# dwm官方默认终端 st
git clone https://git.suckless.org/st --depth=1

# config.mk 文件
#X11INC = /usr/X11R6/include
#X11LIB = /usr/X11R6/lib
# 以上两行改成下面两行
X11INC = /usr/include/X11
X11LIB = /usr/include/X11

sudo make clean install
```

> 如果想开机自启动dwm

```shell
# 全局
vim /etc/X11/xinit/xinitrc 

个别用户： 
vim ~/.xinitrc
# 添加dwm
exec gnome-session
exec dwm   

# 启动
startx
```







### picom

> [picom](https://github.com/pijulius/picom)动画，圆角，模糊，透明  [up](https://yaocc.cc/2022/06/19/linux%E4%B8%9D%E6%BB%91%E7%9A%84%E5%8A%A8%E7%94%BB%E4%BD%93%E9%AA%8C%E2%80%94%E2%80%94picom/)

```shell
git clone https://github.com/yaocccc/picom.git

cd picom
# 切换到分支
git checkout implement-window-animations

# 执行
rm -rf build
LDFLAGS="-L/usr/local/lib" CPPFLAGS="-I/usr/local/include" meson --buildtype=release . build
ninja -C build
sudo ninja -C build install

# 报错的话依赖缺少
sudo pacman -S meson

# 启动
picom --experimental-backends --config ~/scripts/config/picom.conf
```

> 配置文件[picom](https://github.com/EdenQwQ/dots/blob/master/.config/picom.conf)








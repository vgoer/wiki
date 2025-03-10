---
title: Archcraft OS
description: archcraft linux
published: 1
date: 2023-06-09T10:10:35.671Z
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
https://zhuanlan.zhihu.com/p/393746270
yay -S rime-cloverpinyino
```

> 崭新的fcitx5安装 [archwiki](https://wiki.archlinuxcn.org/wiki/Fcitx5?rdfrom=https%3A%2F%2Fwiki.archlinux.org%2Findex.php%3Ftitle%3DFcitx5_%28%25E7%25AE%2580%25E4%25BD%2593%25E4%25B8%25AD%25E6%2596%2587%29%26redirect%3Dno)

```shell
# fcitx5 套件
sudo pacman -S fcitx5-im

# 中文输入模块
sudo pacman -S fcitx5-chinese-addons

# 安装 zhwiki词库 和 搜狗词库
sudo pacman -S fcitx5-pinyin-zhwiki 
yay -S  fcitx5-pinyin-sougou 

# waylad下配置
系统设置 > 输入设备 > 虚拟键盘，选择 Fcitx 5。

# 配置
sudo vim /etc/environment

# wayland
XMODIFIERS=@im=fcitx

# x11
# GTK_IM_MODULE=fcitx
# QT_IM_MODULE=fcitx
# SDL_IM_MODULE=fcitx

```



7. 软件 [up](https://linux265.com/news/3544.html)

```shell
sudo pacman -S google-chrome 

# 设置默认浏览器
unset BROWSER
xdg-settings set default-web-browser google-chrome.desktop

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
sudo  pacman -S  v2rayA

# 更好的代码
yay -S clash-for-windows-bin

# 推荐
yay -S clash-verge-rev-bin
```

> clash: [github](https://github.com/clash-verge-rev/clash-verge-rev)



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


13. 源码安装
```shell
    # 有时候包管理安装不上去，源码编译按照
    https://digitalixy.com/linux/522223.html
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

# 其他软件与carccraft有关的
sudo pacman -Ss archcraft # 老哥出品必属精品。
```



> steam: 有steam就够了
>
> 游戏看 另外一篇文章

```shell
yay -S steam
 yay -S reshade-steam-proton-git  # steam play 
```












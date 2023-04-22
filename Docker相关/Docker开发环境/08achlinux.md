---
title: 08.achlinux
description: achlinux
published: 1
date: 2023-04-22T05:54:45.992Z
tags: docker env, \
editor: markdown
dateCreated: 2023-04-22T05:54:45.992Z
---

<center>arch</center>



[toc]







## Arch

> docker 里面安装archlinux



### 1. 安装

```shell
docker pull archlinux

# vnc 默认 5900
docker run -itd -p 5901:5901 archlinux

docker exec -it dockerid bash
```



### 2.换源

```shell
echo '
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
'  > /etc/pacman.d/mirrorlist
echo '
[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
' >> /etc/pacman.conf
```

```shell
# GPG key
pacman -Sy
pacman -Sy archlinuxcn-keyring
pacman -S git gcc make vim neofetch yay

pacman-key --init
pacman-key -r F9F9FA97A403F63E
pacman-key --lsign-key F9F9FA97A403F63E

#清理缓存
pacman -Scc
# 升级
pacman -Syyu

```



### 3. vnc 

> 远程server

```shell
yay -S tigervnc

vncpasswd #设置密码

#需要什么环境就设置，前提要安装好
echo 'session=dwm' >> ~/.vnc/config
```

>  启动

```shell
# 启动命令
vncserver :1

# 也可以在docker外运行这个命令
docker exec -u root -d arch bash -c '/usr/sbin/vncserver :1'
```

> 桌面端下载

> [vnc](https://www.realvnc.com/en/connect/download/viewer/)

```shell
# vnc  
ip:5901
```



### kde

> kde环境

```shell
# kde 
pacman -S xorg 
# docker不用sddm登录界面
pacman -S sddm 
systemctl enable sddm	#  设置开机自启
pacman -S plasma konsole  # kde-applications：KDE 的全套应用（软件太多了，很多用不到，在此不安装）

mkdir -p /usr/share/xsessions

# ked 
echo '[Desktop Entry]
Encoding=UTF-8
Name=plasma
Comment=Dynamic window manager
Exec=plasma
Icon=plasma
Type=XSession' > /usr/share/xsessions/plasma.desktop
```






































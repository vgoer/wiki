---
title: i3wm
description: i3wm桌面环境
published: 1
date: 2023-04-24T10:27:42.091Z
tags: desktop environment, i3wm
editor: markdown
dateCreated: 2023-04-18T17:32:10.340Z
---

<center>i3wm</center>



[toc]





## i3wm 

> 平铺桌面环境 [i3](https://github.com/i3/i3)





### 1. 安装

debian

```shell
sudo apt install i3 
```

arch

```shell
sudo pacamn -S i3
```



### 2.快捷键

```shell
# 进来就 设置你的super键  //  显示键盘的软件 screenkey
# win or alt
alt + enter 打开一个终端

shif +alt + q 关闭终端

alt + v 横向 创建终端

alt + h 纵向 创建终端

shift + alt + 左右方向  移动该窗口

alt + s 窗口重叠 

alt + 上下方向 切换重叠窗口

alt + w 窗口标签页重叠 

alt + 上下方向 切换标签页重叠窗口

alt  + shift +  悬浮窗口

alt + f 全屏

# 修改工作区
alt + 123456

alt + r + 方向 修改窗口大小

alt + R  刷新配置文件

shift + alt 工作区， 移动窗口到该窗口区
```



### 3. 配置文件

> 配置：`~/.confi/i3/config`

```shell
# super 键
set $mod Mod1

# 字体
font pango:monospace 8


# 开机启动配置
exec --no-startup-id xss-lock --transfer-sleep-lock -- i3lock --nofork

#指定 快捷键  执行一个终端命令
# start a terminal
bindsym $mod+Return exec i3-sensible-terminal

# kill focused window
bindsym $mod+Shift+q kill
```



### 4. 美化



1. 状态栏

```shell
# 删除自带的bar
sudo apt remove i3status

sudo apt install polybar
sudo pacman -S polybar 

sudo cp /usr/share/doc/polybar/examples/config ~/.config/polybar/config

# 列子 
polybar example

// 添加主题
 git clone --depth=1 https://github.com/adi1090x/polybar-themes.git
 cd polybar-themes
 chmod +x setup.sh

bash ~/.config/polybar/launch.sh --hack  # --hack 添加安装的主题

添加到i3配置文件
exec_always --no-startup-id $HOME/.config/polybar/launch.sh --hack
```

> polybar 配置:[配置](https://zhuanlan.zhihu.com/p/385584238) 
>
> theme:[theme](https://github.com/adi1090x/polybar-themes) ,[theme2](https://github.com/zunpeng/polybar)



> 安装依赖 polybar

```shell
Polybar : Ofcourse, the bar itself
Rofi : For App launcher, network, power and style menus
pywal : For pywal support
calc : For random colors support
networkmanager_dmenu : For network modules
```

```shell
# 背景图片
bash ~/.config/polybar/material/scripts/pywal.sh /path/to/wallpaper  # 背景图片

# 随机颜色
bash ~/.config/polybar/material/scripts/random.sh
```





2. 快速启动软件 [rofi ](https://github.com/davatorium/rofi) 

> 主题[theme](https://github.com/davatorium/rofi-themes)

```shell
rofi 

sudo pacman -S rofi
# 启动
rofi -show run 

# 配置
mkdir -p ~/.config/rofi
rofi -dump-config > ~/.config/rofi/config.rasi
# 查看官方主题
rofi-theme-selector 

# 下载用户主题  https://github.com/davatorium/rofi-themes
配置文件下新建主题文件  
vim ***.rasi
# 查看配置文件
rofi options -theme **.rasi -show drun
# 指定图标
rofi -theme material -font "hack 20" -show drun -show-icons

# 配置i3文件
bindsym $mod+d exec rofi -theme material -font "hack 20" -show drun -show-icons
```

> rofi 自定义脚本[script](https://github.com/davatorium/rofi/blob/next/doc/rofi-script.5.markdown)

```shell
rofi -show fb -modi "fb:file_browser.sh"

vim add.sh 

#!/bin/bash
echo 1
echo 2
echo 3
echo 4

# 添加执行权限
chmod +x add.sh

# 执行就可以看到我们的菜单了
rofi -show goer -modi "goer:~/.config/rofi/add.sh"
```

> 菜单脚本

```shell
case "$*" in 
	"java")
	notify-send "java"
		echo 'yes'
		echo 'no'
		;;
	"js")
	notify-send "java"
		;;
	"goer")
	notify-send "goer"	
		;;
	"")
	echo java
	echo js
	echo goer
	;;
esac
```






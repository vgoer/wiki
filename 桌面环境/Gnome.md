---
title: Gnome
description: gnome 桌面环境
published: 1
date: 2023-04-18T17:28:56.991Z
tags: desktop environment, gnome
editor: markdown
dateCreated: 2023-04-18T17:28:56.991Z
---

<center>gnome</center>



> gnome 配置优化
> 
> 新大陆 [gnome-look](https://www.gnome-look.org/browse/)
> 
> pling[pling](https://www.pling.com/) 主题背景等等

```shell 
# gnome 工具
sudo apt install gnome-tweak-tool gnome-shell-extensions chrome-gnome-shell
```





## 美化

### 1. 图标

> 直接去gnome-look icon themes 下载想要的图标

```shell
# 将图标移动到 移动到 /usr/share/icons 目录下。
mv ** /usr/share/icons

# tar.xz 解压方法
# https://blog.csdn.net/yjk13703623757/article/details/79848878

# 打开优化工具ok
```

### 1.主题
> 里面找 [gnome-look](https://www.gnome-look.org/browse?cat=135&ord=latest)

```shell
# 将图标移动到 移动到 /usr/share/icons 目录下。
mv ** /usr/share/theme

不错的
美化：https://yaozhijin.gitee.io/Ubuntu20-04%E7%BB%88%E6%9E%81%E7%BE%8E%E5%8C%96.html
theme: https://zhuanlan.zhihu.com/p/62367160
0. Orchis gtk theme
1. Sweet
2. hooli 
3. Big Sur - OE2
4. Rounded Rectangle Purple Shell Theme

```


### 4. GNOME Shell 扩展

> 关于GNOME的[blog](https://zhuanlan.zhihu.com/p/71588449)
> 
> 扩展 [gnome](https://extensions.gnome.org/)

1. Burn-My-Windows
> 让 窗口着火 [Burn-My-Windows](https://github.com/Schneegans/Burn-My-Windows)
> 
> 效果太帅了


```shell

#这里我下在稳定版
1. 下载
wget https://github.com/Schneegans/Burn-My-Windows/releases/latest/download/burn-my-windows@schneegans.github.com.zip

2. 安装
gnome-extensions install burn-my-windows@schneegans.github.com.zip

3.启动
gnome-extensions enable burn-my-windows@schneegans.github.com

4. 注销下就有效果了
5. 好像其他发行版也可以 
https://wiki.gnome.org/Projects/GnomeShellIntegrationForChrome/Installation
```
> 配置 打开 扩展设置   就有了


2. Dash to Dock 
> dack 栏 [Dash to Dock ](https://github.com/micheleg/dash-to-dock/)

> 下载 [dock](https://extensions.gnome.org/extension/307/dash-to-dock/)

```shell

gnome-extensions install  **.zip

gnome-extensions 插件管理的 
gnome-extensions list 查看插件
```

3. User Themes 
> 主题插件 [User Themes](https://extensions.gnome.org/extension/19/user-themes/)

```shell
 下载安装配置 和 上面一样
```

4. Fly-Pie
>  标记菜单   1作者的另一个项目 [fly pie](https://github.com/Schneegans/Fly-Pie)

5. Desktop-Cube
> 3d桌面  1作者的另一个项目 我只能说牛逼 [Desktop-Cube](https://github.com/Schneegans/Desktop-Cube/)






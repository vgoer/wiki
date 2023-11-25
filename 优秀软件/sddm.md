<center>sddm</center>





[toc]







## Sddm

> SDDM 是 X11 和 Wayland 会话的现代显示管理器
>
> [sddm](https://github.com/sddm/sddm)









### 1. 安装

```shell
yay -S sddm
```





### 2. 配置

> [corners](https://github.com/aczw/sddm-theme-corners) 
>
> [chili](https://github.com/MarianArlt/sddm-chili)
>
> [sddm](https://github.com/catppuccin/sddm)
>
> [sugar](https://github.com/MarianArlt/sddm-sugar-light)
>
> [delicious](https://github.com/stuomas/delicious-sddm-theme)`
>
> [gokyo](https://github.com/rototrash/tokyo-night-sddm)

```shell
pacman -Syu sddm qt5-graphicaleffects qt5-svg qt5-quickcontrols2

git clone https://github.com/aczw/sddm-theme-corners.git
cd sddm-theme-corners
cp -r corners/ /usr/share/sddm/themes/
```

>创建一个`/etc/sddm.conf.d/`
>
>```shell
>[Theme]
>Current=corners
>```
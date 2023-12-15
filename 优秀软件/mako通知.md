<center>mako</center>





[toc]





## mako

> mako 是 Wayland 合成器的图形通知守护进程 [github](https://github.com/emersion/mako)
>
> wayland扩展： [wayland](https://wayland.emersion.fr/)



### 1.安装

```shell
yay -S mako
```





### 2. 配置

```shell
font=Fira Code Nerd Font Mono 24
border-size=2
margin=2,4
padding=2

width=700
height=300
max-icon-size=128
default-timeout=10000

background-color=#2e344099
text-color=#e5e9f0
border-color=#ebcb8B

[urgency=low]
background-color=#2e344099
text-color=#5e81ac
border-color=#ebcb8b

[urgency=normal]
# on-notify=exec mpv ~/sounds/notification.ogg

[urgency=high]
background-color=#2e3440
text-color=#88c0d0
border-color=#ebcb8b
on-notify=exec mpv ~/sounds/notification.ogg

[category=music-notify]
default-timeout=5000
```






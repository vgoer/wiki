<center>rofi启动器</center>







[toc]







## rofi启动器

> rofi–一个窗口切换器、应用启动器、dmenu的替代品 [github ](https://github.com/davatorium/rofi#modes)



## 1. 安装

> rofi: 文章[cc](https://yaocc.cc/rofimenu/)

```shell
yay -S rofi
```

```shell
# 命令
rofi -show 自定义 -modi "自定义:~/rofi.sh"
  1: 上述命令可调用rofi.sh作为自定义脚本
  2: 将打印的内容作为rofi的选项
  3: 每次选中后 会用选中项作为入参再次调用脚本
  4: 当没有输出时 整个过程结束
```

> `rofi.sh`

```shell
case "$*" in
    "poweroff") 
        notify-send "Shutting down"
        echo yes
        echo no
        ;;
    "reboot")
        notify-send "reboot"
        ;;
    "lock")
        notify-send "lock"
        ;;

    "yes")
        notify-send "已触发关机"
        ;;
    "")
        echo poweroff
        echo reboot
        echo lock
        ;;
esac
```

```shell
# 执行
 rofi -show powermenu -modi "powermenu:~/shell/myshell/rofi.sh"
```





### 2. dmenu

> rofi dmenu模式 [wiki](https://github.com/davatorium/rofi/blob/next/doc/rofi-dmenu.5.markdown)

```shell
file=$(ls | rofi -dmenu -window-title find)
echo $file

# 指定主题
history | rofi -dmenu -config ~/.config/rofi/clipboard.rasi

# fzf
h=$(history | fzf) 
```




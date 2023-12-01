<center>notify-send</center>





[toc]





## notify-send

> 桌面通知。 [arch](https://man.archlinux.org/man/notify-send.1.en)





## 1. 安装

```shell
# arch 
yay -S notify-send
```





### 2. 使用

```shell
notify-send -u critical -t 1000 "goer"
-u 紧急程度  low, normal, critical 
-t 信息停留时间 毫秒
-i 图标 

#标题和副标题
notify-send "我是标题" "我是内容，测试用的通知。"

# 指定屏幕
如果你有多个屏幕，使用 DISPLAY=:0.0 可以给第一个屏幕发送通知，以此类推，详细自己百度，我只有一个屏幕，笑。

# 执行命令
notify-send "当前目录是 `pwd`"

# 升级
DISPLAY=:0.0 notify-send -t 3000 "系统升级...." && sudo apt-get upgrade -y && notify-send -t 3000 "升级完成。"

DISPLAY=:0.0 notify-send -t 3000 "系统升级...." && yay -Syyu && notify-send -t 3000 "升级完成。"
```
























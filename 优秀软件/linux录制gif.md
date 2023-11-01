<center>Linux 录制gif</center>



[toc]







## Linux 录制gif

> [byzanz](https://github.com/xatgithub/byzanz) 是一个轻量的 用于录制gif的命令行工具，!!!无GUI!!! [yaocc](https://yaocc.cc/linuxgifsolution)



## byzanz的简单使用示例

```shell
byzanz-record -x 0 -y 0 -w 1920 -h 1080 -d 10 --delay=3 -v ~/demo.gif --exec="some command"
# 参数介绍
-x 录制位置起始点x
-y 录制位置起始点y
-w 录制宽度
-h 录制高度
-d 录制持续时长
--delay 延迟任意秒后开始录制
-v 保存为指定文件
--exec 录制某某命令期间

例如想延迟3s录制整个屏幕10s时长的gif可以用命令
byzanz-record -x 0 -y 0 -w 1920 -h 1080 -d 10 --delay=3 -v ~/demo.gif
```





## 固化脚本使用

命令行下是可以直接通过命令的形式, 但非常不方便 此时我们可以通过固化为一个脚本来使用命令
`~/gif-recorder.sh`

> ```shell
> #! /bin/bash
> byzanz-record -x 0 -y 0 -w 1920 -h 1080 -d 10 --delay=3 -v ~/demo.gif
> ```
>



## 符合GUI的录制操作

此时可以借助一些别的工具来获取 xywh 值, 例如
利用 xwininfo 命令获取指定窗口的 x y w h 值
利用 xrectsel 命令获取指定区域的 x y w h 值
若没有这两个包可以手动安装

```shell
#! /bin/bash

gif_file=~/demo.gif
let x y w h

getwin() {
    XWININFO=$(xwininfo)
    read x < <(awk -F: '/Absolute upper-left X/{print $2}' <<< "$XWININFO")
    read y < <(awk -F: '/Absolute upper-left Y/{print $2}' <<< "$XWININFO")
    read w < <(awk -F: '/Width/{print $2}' <<< "$XWININFO")
    read h < <(awk -F: '/Height/{print $2}' <<< "$XWININFO")
}
getregion() {
    xywh=($(xrectsel "%x %y %w %h")) || exit -1
    let x=${xywh[0]} y=${xywh[1]} w=${xywh[2]} h=${xywh[3]}
}

case $1 in
    1) getwin ;;
    2) getregion ;;
    3) let x=0 y=0 w=1920 h=1080 ;;
    *)
        echo 1: 选择窗口
        echo 2: 选择区域
        echo 3: 全屏
        exit
        ;;
esac

[ "$2" ] \
    && byzanz-record -x $x -y $y -w $w -h $h -v $gif_file --exec="$2 $3 $4 $5 $6 $7 $8 $9 $10" \
    || byzanz-record -x $x -y $y -w $w -h $h -v $gif_file
sleep 1
```

!!!上述脚本简单解析!!!

```shell
getwin 方法用于获取窗口的 x y w h 值
getregion 方法用于获取指定区域的 x y w h 值

!!!使用!!!: ./gif-recorder.sh $模式 $命令
上述脚本 固化了 三个模式
  1 选择窗口
  2 选择区域
  3 指定录制全屏
例如使用命令
./gif-recorder.sh 1 vim README.md
即可手动指定某窗口 并录制 vim 这个命令直到退出vim

也可以自己选择性添加参数进行定制操作
```




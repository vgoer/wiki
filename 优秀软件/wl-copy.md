<center> wl-copy</center>





[toc]





## wl-copy

> wayland环境下的剪切板工具 [github](https://github.com/bugaevc/wl-clipboard)





### 1. 安装

```shell
 yay -S wl-clipboard
```





### 2. 使用

> 该项目实现了两个命令行 Wayland 剪贴板实用程序`wl-copy` 和`wl-paste`，使您可以轻松地在剪贴板和 Unix 管道、套接字、文件等之间复制数据。

```shell
# 复制一条简单的短信：
$ wl-copy 世界你好！

# 复制 ~/Downloads 中的文件列表：
$ ls ~/下载| wl-复制

# 复制图像：
$ wl-copy < ~/Pictures/photo.png

# 复制之前的命令：
$ wl-copy "!!"

# 粘贴到文件：
$ wl-paste > clipboard.txt

# 对剪贴板内容进行排序：
$ wl-paste | sort | wl-copy

# 每次更改时将剪贴板内容上传到 Pastebin：
wl-paste --watch nc paste.example.org 5555
```






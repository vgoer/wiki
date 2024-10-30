<center>kitty终端</center>





[toc]





## kitty

> 跨平台、快速、功能丰富、基于 GPU 的终端 [github](https://github.com/kovidgoyal/kitty)





### 1. 安装

```shell
yay -S kitty-git
```





### 2. 配置

> `.confg/kitty` 配置文件
>
> [theme](https://github.com/dexpota/kitty-themes) [theme1](https://github.com/catppuccin/kitty)

```shell
kitty 
```





### 3. neovide类似光标配置

> [github](https://github.com/kovidgoyal/kitty/releases)   [reddit](https://www.reddit.com/r/KittyTerminal/comments/1g7vkwt/neovide_like_cursor_animation_in_kitty_terminal/)  [bilibili](https://www.bilibili.com/video/BV1Kw1xYqEpy)



```shell
# 下载
curl -L https://sw.kovidgoyal.net/kitty/installer.sh | sh /dev/stdin \
    installer=nightly
```



```shell
# 添加配置
cursor_trail 3
```


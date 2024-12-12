<center>cliphist剪切板</center>





[toc]







## cliphist剪切板

> 支持多媒体的 Wayland 剪贴板管理器
>
> [github](https://github.com/sentriz/cliphist)









### 1. 安装

```shell
# 可以使用go path安装
go install go.senan.xyz/cliphist@latest

# arch
yay -Ss cliphist 
```





### 2. 用法

```shell
用法
监听剪贴板的变化
$ wl-paste --watch cliphist store
这将监听主剪贴板上的更改并将其写入历史记录。
每个会话调用一次 - 例如在您的 sway 配置中

选择旧项目
$ cliphist list | dmenu | cliphist decode | wl-copy
将它绑定到键盘上的某个好东西上

删除旧项目
$ cliphist list | dmenu | cliphist delete
或者手动查询
$ cliphist delete-query "secret item"

清除数据库
$ cliphist wipe
```





### 3.选择器

```shell
选择器示例
菜单
cliphist list | dmenu | cliphist decode | wl-copy

韋茨夫
cliphist list | fzf --no-sort | cliphist decode | wl-copy

rofi（dmenu 模式）
cliphist list | rofi -dmenu | cliphist decode | wl-copy

rofi（自定义模式）
rofi -modi clipboard:/path/to/cliphist-rofi -show clipboard

（需要contrib/cliphist-rofi）

rofi（带图像的自定义模式）
rofi -modi clipboard:/path/to/cliphist-rofi-img -show clipboard -show-icons

（需要contrib/cliphist-rofi-img）

沃菲
cliphist list | wofi -S dmenu | cliphist decode | wl-copy

摇摆配置示例：
```


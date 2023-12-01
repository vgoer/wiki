<center>grim</center>





[toc]





## grim

> 从 Wayland 合成器中获取图像 [github](https://github.com/emersion/grim)
>
> [git](https://git.sr.ht/~emersion/grim/refs)





## 1. 安装

```shell
# arch 
yay -S grim
```





### 2. 使用

```shell
# 截图所有输出
grim

# 截图一个输出
grim -o DP-1

# 区域截图
grim -g "10,20 300x400"

# 配合slurp
grim -g "$(slurp)"

# 自定义名称
grim $(xdg-user-dir PICTURES)/$(date +'%s_grim.png')

# 截图到剪切板
grim -g "$(slurp)" -  | wl-copy

# swaymsg使用和 从 Sway 下的聚焦监视器中获取屏幕截图jq
grim -o $( swaymsg -t get_outputs | jq -r '.[] | select(.focused) | .name' )

grim -g " $( swaymsg -t get_tree | jq -j '.. | select(.type?) | select(.focused).rect | "\(.x),\(.y) \(.width) x\(.height)"' 

# 获取颜色 
grim -g "$(slurp -p)" -t ppm - | convert - -format '%[pixel:p{0,0}]' txt:-
```




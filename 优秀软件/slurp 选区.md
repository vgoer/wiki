<center>slurp选区</center>





[toc]





## slurp 

> 在 Wayland 合成器中选择一个区域  [github](https://github.com/emersion/slurp)





### 1. 安装

```shell
# 编译
git clone https://github.com/emersion/slurp
cd slurp
meson setup build
ninja -C build
build/slurp

# arch 
yay -S slurp
```





### 2. 使用

```shell
# 获取选区
slurp

# 选择单个点
slurp -p

# 选择一个输出 打印其名称
slurp -o -f "%o"

# swaymsg使用和选择 Sway 下的窗口jq：
swaymsg -t get_tree | jq -r '.. | select(.pid? and .visible?) | .rect | "\(.x),\(.y) \(.width)x\(.height)"' | slurp
```


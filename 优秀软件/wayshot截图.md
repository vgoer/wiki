<center>wayshot截图</center>





[toc]





## wayshot 截图

> 用于基于 wlroots 的合成器的屏幕截图工具 [git](https://git.sr.ht/~shinyzenith/wayshot)







## 1.安装

```shell
# 1. 编译
git clone https://github.com/waycrate/wayshot && cd wayshot
make setup
make
sudo make install

# arch 
yay -S wayshot
```





### 2. 使用

```shell
# 全屏
wayshot

# 选区截图
wayshot -s "$(slurp)"

# 保存到剪切板
 wayshot -s "$(slurp)" --stdout | wl-copy
 
# 获取图片颜色代码
wayshot -s "$(slurp -p)" --stdout | convert - -format '%[pixel:p{0,0}]' txt:-|grep -E "#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})" -o
```


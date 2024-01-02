<center>Eww</center>







[toc]





## Eww

> eww: [eww](https://github.com/elkowar/eww) 古怪的小部件 [doc](https://elkowar.github.io/eww/)





### 1. 安装

```shell
# 源码 和 构建
git clone https://github.com/elkowar/eww

然后构建：

cargo build --release --no-default-features --features x11

注意： 当您在 Wayland 上时，请使用以下命令进行构建：

cargo build --release --no-default-features --features=wayland
```

> arch

```shell
# x11
yay -S eww-x11 

# wayland
yay -S eww-wayland
```







### 2 . 运行

> run

```shell
chmod +x ./eww

./eww daemon
./eww open <window_name>
```

```shell
# arch
eww 
```



### 3.  配置

> 首先，您需要创建两个文件：`eww.yuck`和`eww.scss`（或者`eww.css`，如果您愿意的话）。这些文件必须放在`$XDG_CONFIG_HOME/eww`（这很可能`~/.config/eww`）下。

```yaml
(defwindow example
           :monitor 0
           :geometry (geometry :x "0%"
                               :y "20px"
                               :width "90%"
                               :height "30px"
                               :anchor "top center")
           :stacking "fg"
           :reserve (struts :distance "40px" :side "top")
           :windowtype "dock"
           :wm-ignore false
"example content")

```

> 在这里，我们定义一个名为 的窗口`example`，然后为其定义一组属性。此外，我们将窗口的内容设置为 text `"example content"`。
>
> 您现在可以通过运行打开第一个窗口`eww open example`！辉煌！

```shell
# 运行
eww open example 

eww close example
```
















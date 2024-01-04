<center>waybar</center>





[toc]





## waybar

> 高度可定制的Wayland酒吧，适用于基于Sway和Wlroots的合成器。✌️ 🎉 [waybar](https://github.com/Alexays/Waybar)







### 1.安装

> install
>
> [blog](https://cascade.moe/posts/hyprland-configure/)

```shell
yay -S waybar-git
```





### 2 .配置 Waybar

Waybar 拥有**两个主要的配置文件**，它们分别是：

- `~/.config/waybar/config`: 一个 JSON 文件，配置 Waybar 显示的**内容**等。
- `~/.config/waybar/style.css`: 一个 CSS 文件，配置 Waybar 的**样式**。

```shell
{
    "layer": "top",     // 显示的层，top 为最顶层
    "position": "top",  // 在屏幕的哪边显示，top 为顶部
    "height": 40,       // 高度
    "spacing": 6,       // 各 module 之间的距离
    // 左边的 modules
    "modules-left": ["wlr/workspaces", "hyprland/window"],
    // 正中间的 modules
    "modules-center": [],
    // 最右边的 modules
    "modules-right": ["tray", "network", "pulseaudio", "memory", "cpu", "clock"],
    // module 各自的配置等
    "wlr/workspaces": {
        // ...
    }
    // ...
}
```

> 启动

```shell
waybar
```


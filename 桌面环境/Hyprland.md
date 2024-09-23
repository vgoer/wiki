<center>Hyprland</center>





[toc]





## Hyprland

> 一款基于 wlroot 的动态拼贴韦兰排字工具，不会因为外观而有所牺牲。[hypr](https://hyprland.org/) [awesome](https://github.com/hyprland-community/awesome-hyprland) [theme](https://community.hyprland.org/themes.html) [blog](https://blog.cascade.moe/posts/hyprland-configure/) [zhihu ](https://www.zhihu.com/question/569405567)[探索笔记 ](https://samwhelp.github.io/note-about-hyprland/)  [wayland工具](https://wayland.emersion.fr/)
>
> [wayland软件](https://arewewaylandyet.com/) [github](https://github.com/mpsq/arewewaylandyet)
>
> 配置详情： [config](https://cascade.moe/posts/hyprland-configure/)



```shell
# 老哥已经配置好了，直接安装就行。 感谢
https://github.com/prasanthrangan/hyprdots
# 看步骤，超级简单的。

# Hyde cil 工具
yay -S hyde-cli-git

# 安装主题
Hyde theme patch
Hyde sddm list
```



> hyprland详细介绍
>
> [wiki](https://wiki.hyprland.org/)

1. 安装

`````shell
# arch
yay -S hyprland-git
`````

2. 监视器

```shell
monitor = DP-1, 1920x1080@144, 0x0, 1

这将使显示器DP-1以1920x1080144Hz 的频率0x0从左上角偏离，比例为 1（未缩放）。
```

3. 绑定

```shell
bind = MODS, key, dispatcher, params

bind = SUPER_SHIFT, Q, exec, firefox

$mainMod = SUPER # windows key
bind = $mainMod, E, exec, $file # open file manager
bind = $mainMod, D, exec, $editor # open vscode
bind = $mainMod, F, exec, $browser # open browser
```

4. 输入

````shell
input {
    kb_layout = us              # 键盘布局
    follow_mouse = 1            # 窗口焦点是否随光标移动
    touchpad {
        natural_scroll = no     # 触摸板自然滚动
    }
    sensitivity = 0             # 鼠标灵敏度
    accel_profile = flat        # 鼠标加速的配置方案, flat 为禁用鼠标加速
}
````

5. 总体设置

`````shell
general {
    gaps_in = 6         # 窗口之间的间隙大小
    gaps_out = 12       # 窗口与显示器边缘的间隙大小
    border_size = 2     # 窗口边框的宽度
    col.active_border = rgba(cceeffbb)      # 活动窗口的边框颜色 
    col.inactive_border = rgba(595959aa)    # 非活动窗口的边框颜色

    layout = dwindle    # 窗口布局
}
`````

6. 窗口装饰

```shell
decoration {
    rounding = 12       # 圆角大小
    blur = yes          # 模糊效果是否启用
    blur_size = 5       # 模糊半径
    blur_passes = 1     # 模糊过滤次数
    blur_new_optimizations = on     # 模糊优化，通常保持打开

    drop_shadow = yes   # 窗口投影是否启用
    shadow_range = 4    # 投影大小
    shadow_render_power = 3     # 投影强度，不过我不太明白这是什么意思
    col.shadow = rgba(1a1a1aee)     # 投影颜色
}
```

7. 动画

```shell
animations {
    enabled = yes   # 是否启用动画

    bezier = myBezier, 0.05, 0.9, 0.1, 1.05     # 自定义的贝塞尔曲线

    animation = windowsMove, 1, 7, myBezier             # 窗口移动动画
    animation = windowsIn, 1, 3, default, popin 90%     # 窗口打开动画
    animation = windowsOut, 1, 3, default, popin 90%    # 窗口关闭动画
    animation = border, 1, 2, default       # 边框颜色动画
    animation = fade, 1, 3, default         # 窗口各种 Fade 效果（打开、关闭、阴影、活动与非活动窗口切换）的动画
    animation = workspaces, 1, 3, default   # 工作区切换动画
}
```









### 2. 桌面通知

> ==notify-send==  桌面通知  [blog](https://blog.cascade.moe/posts/hyprland-configure/)

```shell
# install 
sudo pacman -Sy libnotify

# 使用
notify-send -u critical 'Hi goer!' 'Welcome, you are my BOOS.' -u normal  

```





### 3. 截图和操作图片

> 截图： [grim](https://mephisto.cc/tech/wayland-screenshot/)

```shell
grim -g "`slurp`" - | ksnip -
```







### 4. waybar

> [Waybar](https://github.com/Alexays/Waybar) 为 Wayland 提供了一个高度可定制的状态栏。

```shell
# shell
yay -S waybar-git
```

#### 配置 Waybar

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







### 5. 动态壁纸

> hyprland动态壁纸

```shell
yay -S mpvpaper-git

mpvpaper eDP-1 ~/bg/xxx.mp4 -o " --loop " -f --no-audio
```






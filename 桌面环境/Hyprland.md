<center>Hyprland</center>





[toc]





## Hyprland

> 一款基于 wlroot 的动态拼贴韦兰排字工具，不会因为外观而有所牺牲。[hypr](https://hyprland.org/) [awesome](https://github.com/hyprland-community/awesome-hyprland) [theme](https://community.hyprland.org/themes.html) [blog](https://blog.cascade.moe/posts/hyprland-configure/) [zhihu ](https://www.zhihu.com/question/569405567)[探索笔记 ](https://samwhelp.github.io/note-about-hyprland/)  [wayland工具](https://wayland.emersion.fr/)
>
> [wayland软件](https://arewewaylandyet.com/) [github](https://github.com/mpsq/arewewaylandyet)
>
> 配置详情： [config](https://cascade.moe/posts/hyprland-configure/)
>
> pyprpanel: [hyprpanel](https://hyprpanel.com/)
>
> awesome-hyprland: [github](https://github.com/hyprland-community/awesome-hyprland)
>
> nwg: [nwg](https://github.com/nwg-piotr)



```shell
# 老哥已经配置好了，直接安装就行。 感谢
https://github.com/prasanthrangan/hyprdots
# 看步骤，超级简单的。

# Hyde cil 工具
yay -S hyde-cli-git

# 安装主题 discord 上面很多主题。
Hyde theme patch
Hyde sddm list

# telegram 主题
https://github.com/HyDE-Project/telegram-hyde.git
```

1. 将文件添加`/telegram.dcol`到以下任一目录：

- `~/.config/hyde/wallbash/Wall-Ways`
- `~/.config/hyde/wallbash/Wall-Dcol`

1. 复制`./wallbash.telegram.sh`到 $PATH
2. 使用电报的 GUI 将`Wallbash.tdesktop-theme`文件添加为主题。

- 导航至**“设置”** > **“聊天设置”** > **“聊天壁纸”** > **“从文件中选择**”` ~/.cache/hyde/landing/Wallbash.tdesktop-theme`



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





> 解决hyprland下的一些缩放输入法问题：[blog](https://sonnycalcr.github.io/posts/resolve-wayland-or-hyprland-chrome-and-jetrains-scale-issue/)

* vscode

```shell
touch /etc/code-flags.conf
```

````shell
--ozone-platform-hint=wayland
--enable-wayland-ime
````

* chrome

```shlel
# chrome-flags.conf

--enable-features=UseOzonePlatform
--ozone-platform=wayland
--enable-wayland-ime
```

然后是一些我们不知道其 flags 文件怎么命名的软件，比如，Obsidian 和 Jetbrains 家的软件，那么，我们可以直接自制一份我们自己的 desktop file 来覆盖系统默认的，这里拿 Obsidian 举例，我们去 `/usr/share/applications` 下去复制一份 obsidian.desktop 文件到 `~/.local/share/applications/` 下面，然后在 Exec 那一行加一些启动参数即可，

```yaml
1Exec=/usr/bin/obsidian %U --enable-features=UseOzonePlatform --ozone-platform=wayland --enable-wayland-ime
```

Jetbrains 家的软件也是一样，加一下它们提供的 wayland 相关的参数即可，

```yaml
1Exec=intellij-idea-ultimate-edition %u -Dawt.toolkit.name=WLToolkit
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





### 6. 扩展

1. hy3

> Hyprland 插件，适用于 i3 / sway 等手动平铺布局。[github](https://github.com/outfoxxed/hy3)

```shell
# hyprland自己的插件管理
hyprpm update

# 安装 hy3
hyprpm add https://github.com/outfoxxed/hy3

hyprpm enable outfoxxed/hy3

```

2. split-monitor-workspaces

> 一个小型的 Hyprland 插件，提供出色的工作区行为 [github](https://github.com/Duckonaut/split-monitor-workspaces)

```shell
hyprpm add https://github.com/Duckonaut/split-monitor-workspaces # Add the plugin repository
hyprpm enable split-monitor-workspaces # Enable the plugin
hyprpm reload # Reload the plugins
```

```shell
.hyprland.conf
# 启动插件
exec-once = hyprpm reload -n
```

```shell
# 配置
# Overview
plugin {
    wslayout {
        default_layout=master
    }
    split-monitor-workspaces {
        count = 5
        keep_focused = 0
        enable_notifications = 0
        enable_persistent_workspaces = 1
    }
}
```

3. hyprNStack

> 用于 N 堆栈平铺布局的 Hyprland 插件 [github](https://github.com/zakk4223/hyprNStack)

```shell
hyprpm add https://github.com/zakk4223/hyprNStack

hyprpm enable hyprNStack
```

```shell
plugin {
  nstack {
    layout {
      orientation=left
      new_on_top=0
      new_is_master=1
      no_gaps_when_only=0
      special_scale_factor=0.8
      inherit_fullscreen=1
      stacks=2
      center_single_master=0
      mfact=0.5
      single_mfact=0.5
    }
  }
}
```

4. Hyprspace

> Hyprland 的工作区概览插件  [github](https://github.com/KZDKM/Hyprspace)

```shell
hyprpm add https://github.com/KZDKM/Hyprspace
hyprpm enable Hyprspace
```

```shell
# keybindings.conf

bind = $mainMod CTRL, W, overview:toggle # wrokspace overview

# so cool
```

> 其他配置看github

5. hypr-dynamic-cursors

> 一个插件，使你的 hyprland 光标更加逼真，还添加了摇动功能

```shell
hyprpm add https://github.com/virtcode/hypr-dynamic-cursors
hyprpm enable dynamic-cursors
```

```shell
# hyprland.conf

plugin{
	# 鼠标特效
    dynamic-cursors{
        enable =true
        # 模式
        # mode = rotate
        mode = tilt
        # mode = stretch
    }

}
```

6. hyprfocus

>  Hyprland 焦点动画插件 [github](https://github.com/pyt0xic/hyprfocus)

```shell
hyprpm add https://github.com/pyt0xic/hyprfocus
hyprpm enable hyprfocus
```

```shell
# hyprland.conf

    hyprfocus {
        enabled = yes
        animate_floating = yes
        animate_workspacechange = yes
        focus_animation = shrink
        # Beziers for focus animations
        bezier = bezIn, 0.5,0.0,1.0,0.5
        bezier = bezOut, 0.0,0.5,0.5,1.0
        bezier = overshot, 0.05, 0.9, 0.1, 1.05
        bezier = smoothOut, 0.36, 0, 0.66, -0.56
        bezier = smoothIn, 0.25, 1, 0.5, 1
        bezier = realsmooth, 0.28,0.29,.69,1.08
        # Flash settings
        flash {
            flash_opacity = 0.95
            in_bezier = realsmooth
            in_speed = 0.5
            out_bezier = realsmooth
            out_speed = 3
        }
        # Shrink settings
        shrink {
            shrink_percentage = 0.95
            in_bezier = realsmooth
            in_speed = 1
            out_bezier = realsmooth
            out_speed = 2
        }
    }
```


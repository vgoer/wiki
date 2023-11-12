<center>Hyprland</center>





[toc]





## Hyprland

> 一款基于 wlroot 的动态拼贴韦兰排字工具，不会因为外观而有所牺牲。[hypr](https://hyprland.org/) [awesome](https://github.com/hyprland-community/awesome-hyprland) [theme](https://community.hyprland.org/themes.html) [blog](https://blog.cascade.moe/posts/hyprland-configure/) [zhihu ](https://www.zhihu.com/question/569405567)[探索笔记 ](https://samwhelp.github.io/note-about-hyprland/)  [wayland工具](https://wayland.emersion.fr/)



```shell
# 老哥已经配置好了，直接安装就行。 感谢
https://github.com/prasanthrangan/hyprdots
# 看步骤，超级简单的。
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





```shell
# shell

```


<center>wshowkeys</center>





[toc]





## wshowkeys

> 在受支持的 Wayland 合成器上在屏幕上显示按键（需要 `wlr_layer_shell_v1`支持）。[git](https://git.sr.ht/~sircmpwn/wshowkeys)





## 1. 安装

```shell
yay -S wshowkeys
```





### 2. 使用

- *-b #RRGGBB[AA]* : 设置背景颜色
- *-f #RRGGBB[AA]* : 设置前景色
- *-s #RRGGBB[AA]* : 设置特殊键的颜色
- *-F font*：设置字体（Pango格式，例如'monospace 24'）
- *-t timeout* : 在清除旧的击键之前设置超时 `fc-list :lang=zh`
- *-a top|left|right|bottom*：将击键锚定到边缘。可以指定两次。
- *-m margin*：设置距最近边缘的边距（以像素为单位）
- *-o输出*：请求wshowkeys显示在指定的输出上（未实现）

```shell
wshowkeys -a bottom -a right -F "SourceHanSansCN-Light.otf 35" -f "#604351" -s "#6FA27D"
```






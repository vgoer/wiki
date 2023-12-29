<center>Ags</center>





[toc]







## Ags

> 可定制且可扩展的 shell [ags](https://github.com/Aylur/ags/)



### 1. 安装

```shell
yay -S aylurs-gtk-shell # or aylurs-gtk-shell-git


# 使用
ags --help
```





### 2. 使用

> wiki: [wiki](https://github.com/Aylur/ags/wiki)  
>
> 语法： js语法



#### a. 第一各小部件

> `~/.config/ags/config.js`首先使用以下内容进行创建：

```js
import Widget from 'resource:///com/github/Aylur/ags/widget.js';

const myLabel = Widget.Label({
    label: 'some example content',
})

const myBar = Widget.Window({
    name: 'bar',
    anchor: ['top', 'left', 'right'],
    child: myLabel,
})

export default { windows: [myBar] }
```






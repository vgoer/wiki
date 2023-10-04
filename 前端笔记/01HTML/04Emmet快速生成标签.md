---
title: 04Emmet快速生成标签
description: 
published: 1
date: 2023-06-27T18:48:16.262Z
tags: 
editor: markdown
dateCreated: 2023-06-27T18:48:14.796Z
---

<center>html快速生成标签</center>





[toc]





## html快速生成标签

> `Emmet` html快速生成标签的插件，vscode自带了插件.

```shell
1. 带id的div标签
div#dome
#dome 默认是div

2. 带class的h2标签
h2.title


3. 带特定属性的标签
a[href="www.baidu.com"]


4. 带有内容的
div{内容}


6. 标签嵌套的
div.box>p

7. 标签兄弟的
div+h3

8. 分组操作符 （）
(div.box>img[alt='aaa'])*2


9. 乘法操作符 * 
div.box*10

10. 自动计数 $ $$两位数
ul>li.box${$$}*10

11. 自动补全单词  >lorem5=>5个单词
ul>li>lorem10*10
```




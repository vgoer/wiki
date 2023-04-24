---
title: 01.vue简介
description: vue简介
published: 1
date: 2023-04-24T10:35:01.154Z
tags: vue2
editor: markdown
dateCreated: 2023-04-24T10:34:57.816Z
---

<center>vue</center>

[toc]

### Vue

> 课程来源:[vue官网]()

> 特点：
>
> 1. 体积小
>
> 2. 更高的运行效率
>
>    > 基于虚拟dom(dom的预处理操作)
>
> 3. 双向数据绑定
>
>    > 开发者不用操作dom对象，投入到业务逻辑
>
> 4. 生态丰富，学习成本低
>
>    > 拥有大量成熟的基于vue的框架和组件



### 安装

[vue](https://cn.vuejs.org/v2/guide/installation.html)

> 生产环境，开发环境

> cdn(内容分发)

> 命令行 > vue 脚手架（先不建议）

### 第一个vue

> 暴露全局变量Vue

```html
<div id="app">
    <h1>{{msg}}</h1>
</div>

//引入vue
<script src="../script/vue.js"></script>
<script>
    // 创建vue应用对象
    var app = new Vue({  //对象参数
        el:"#app",  // 元素
        data:{      // 数据
            msg:'hello Vue!!!',
        }
    })
</script>
```


---
title: 15.vue3和vite配置
description: vue3和vite配置
published: 1
date: 2023-04-24T12:14:16.460Z
tags: vue3
editor: markdown
dateCreated: 2023-04-24T12:14:16.460Z
---



[toc]





## vite





> 安装less

```shell
npm i less -D 

# vite
import path from 'path'

css: {
  preprocessorOptions: {
      less: {
          modifyVars: {
              hack: `true; @import (reference) "${path.resolve("src/assets/css/base.less")}";`,
          },
          javascriptEnabled: true,
      },
  },
},
```

> less常用语法[less](https://blog.csdn.net/weixin_46032850/article/details/107271709)

```js
// vite 配置@为src
  resolve:{
    alias:{
      '@':path.resolve('./src')   // @代替src
    }
  },
```

> 开发配置环境

```shell
在根目录新建三个环节变量
1、开发环境
2、生产环境
3、测试环境

.env.dev
.env.pro
.env.test

.env 内容
VUE_APP_NAME = 'xxxapp'
BASEURL = 'https:/xxx.com'

# 修改package.json
"devview": "vite --mode dev",
"buildpro": "vite build --mode pro"
```



> css 根据系统主题色

```css
:root{
    --color-background: #1b1b1b;
    --white-color-background: #fff;
}

/* 监听操作系统主题模式 */
@media (prefers-color-scheme: dark) {
    body {
        background-color: var(--color-background);
    }
}

@media (prefers-color-scheme: light) {
    body {
        background-color: var(--white-color-background);
    }
}
```
---
title: 15.vue3和vite配置
description: vue3和vite配置
published: 1
date: 2023-05-29T04:15:10.449Z
tags: vue3
editor: markdown
dateCreated: 2023-04-24T12:14:16.460Z
---

[toc]





## vite



### 1. less

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

> 安装 `sass`[scss](https://juejin.cn/post/7239585610862805051)

### 2.开发配置

> 开发配置环境

```shell
在根目录新建三个环节变量
1、开发环境
2、生产环境

.env.development
.env.production

.env.development 内容
# 开发环境配置  env配置不能留空格
ENV='development'
VITE_APP_API_HOST='http://localhost:8888'

.env.production
# 生产环境配置
ENV='production'
VITE_APP_API_HOST='http://localhost:8887'

# 修改package.json
# dev 默认就是development环境
"dev": "vite", 
"build:pro": "vite build --mode production",

# 使用的话就直接读取 修改api功能最多
 baseURL: import.meta.env.VITE_APP_API_HOST,
```



### 3. css 根据系统主题色

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



### 4. 按需引入

> 需要什么引入什么库

```shell
# 自动导入
npm install -D unplugin-vue-components unplugin-auto-import

# 安装UI
npm i element-plus --save
npm i ant-design-vue --save
npm i vant
```

> vite配置

```js
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { AntDesignVueResolver } from 'unplugin-vue-components/resolvers'
  // ...
  plugins: [
    // ...
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
      
// 如果是其他UI
ElementPlusResolver 替换 AntDesignVueResolver,VantResolver
```



### 5. vite内网配置

> 打开内网

```js
  server:{
    host:true,  // 内网
    cors:true,  //为开发服务器配置 CORS , 默认启用并允许任何源
    open:true,  //服务启动时自动在浏览器中打开应用
    port:'8888'
  },
  build:{
    target:"modules",    //浏览器兼容性  "esnext"|"modules"
    outDir: "dist",       //指定输出路径
    cssCodeSplit:true,   //启用/禁用 CSS 代码拆分
  }
```








---
title: 18.Npm发组件
description: Npm发组件
published: 1
date: 2023-04-25T12:11:11.593Z
tags: vue3
editor: markdown
dateCreated: 2023-04-24T12:17:34.861Z
---

<center>Npm组件</center>





[toc]



## npm发组件

> 封装自己的组件，开源给被人用

- 方面使用，任何项目无缝衔接。
- 可作为开源项目，积累经验。



### 1. 环境

```shell
npm init vite@latest

npm install 

npm run dev
```



### 2. 组件封装

1. 小组件

>写一个小组件 

```html
// 封装组件  新建 packages 文件
go-button/index.vue
<template>
    <button class="btn">add</button>
</template>
<script setup></script>
<style scoped>
.btn {
    --glow-color: rgb(217, 176, 255);
    --glow-spread-color: rgba(191, 123, 255, 0.581);
    --enhanced-glow-color: rgb(231, 206, 255);
    --btn-color: rgb(100, 61, 136);
    border: .25em solid var(--glow-color);
    padding: .5em 2em;
    color: var(--glow-color);
    font-size: 15px;
    font-weight: bold;
    background-color: var(--btn-color);
    border-radius: 1em;
    outline: none;
    box-shadow: 0 0 1em .25em var(--glow-color),
        0 0 4em 1em var(--glow-spread-color),
        inset 0 0 .75em .25em var(--glow-color);
    text-shadow: 0 0 .5em var(--glow-color);
    position: relative;
    transition: all 0.3s;
}

.btn::after {
    pointer-events: none;
    content: "";
    position: absolute;
    top: 120%;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: var(--glow-spread-color);
    filter: blur(2em);
    opacity: .7;
    transform: perspective(1.5em) rotateX(35deg) scale(1, .6);
}

.btn:hover {
    color: var(--btn-color);
    background-color: var(--glow-color);
    box-shadow: 0 0 1em .25em var(--glow-color),
        0 0 4em 2em var(--glow-spread-color),
        inset 0 0 .75em .25em var(--glow-color);
}

.btn:active {
    box-shadow: 0 0 0.6em .25em var(--glow-color),
        0 0 2.5em 2em var(--glow-spread-color),
        inset 0 0 .5em .25em var(--glow-color);
}
</style>

// go-input/index.vue
<template>
    <input class="input" type="text" placeholder="Input here...">
</template>

<script setup></script>

<style scoped>
.input {
    border: none;
    padding: 1rem;
    border-radius: 1rem;
    background: #e8e8e8;
    box-shadow: 20px 20px 60px #c5c5c5,
        -20px -20px 60px #ffffff;
    transition: 0.3s;
    color: brown;
    text-shadow: 1px 1px 2px brown;
}

.input:focus {
    outline-color: #e8e8e8;
    background: #e8e8e8;
    box-shadow: inset 20px 20px 60px #c5c5c5,
        inset -20px -20px 60px #ffffff;
    transition: 0.3s;
}
</style>
```



2. Vue插件模式

> 这一步是封装组件中的重点，用到了Vue提供的一个公开方法：install。这个方法会在你使用`Vue.use(plugin)`时被调用，这样使得我们的插件注册到了全局，在子组件的任何地方都可以使用。

```js
//packages目录下新建index.js文件 

// 引入组件
import GoButton from './go-button/index.vue'
import GoInput from './go-input/index.vue'

// 按需引入
export { GoButton,GoInput } 

// 批量注册组件
const install = (Vue) => {
    GoComs.forEach((com) => {
        Vue.component(com.name,com)
    })
}

// 暴露出去
export default {install}  // 批量的引入*
```



3. 打包

> 修改`vite.config.js`

```js
// 新增build属性
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  // 新加的build属性 
  build: {
    // 注意路径不要搞错
    lib: {
      entry: path.resolve(__dirname, 'src/packages/index.js'),
      name: 'Gocoms',
      fileName: (format) => `Gocoms.${format}.js`
    },
    rollupOptions: {
      // 确保外部化处理那些你不想打包进库的依赖
      external: ['vue'],
      output: {
        // 在 UMD 构建模式下为这些外部化的依赖提供一个全局变量
        globals: {
          vue: 'Vue'
        }
      }
    }
  }
})
```

> ​	添加脚本到`package.json` 

```js
	"scripts": {
		"dev": "vite",
		"build": "vite build",
		"serve": "vite preview"
	},
    // 新增打包文件
  "files": ["dist"],
	"main": "./dist/Gocoms.umd.js",
	"module": "./dist/Gocoms.es.js",
	"exports": {
		".": {
			"import": "./dist/Gocoms.es.js",
			"require": "./dist/Gocoms.umd.js"
		}
	},
```

```shell
npm run build
```



4. 注册npm[npm](https://www.npmjs.com/)

```shell
// 关联npm
1. 初始化 package.json 进入dist目录中
npm init -y

2. 修改package.json 
//package.json  name 有唯一性
{
  "name": "gocoms",
  "version": "1.0.2",
  "description": "go 测试组件",
  "main": "Gocoms.es.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "goer",
  "license": "ISC"
}

3. 添加用户
npm adduser  // 用户名密码 code

4. 发布
npm publish  // 发布失败可能是名字重复了，改了名字即可

5. 然后仓库就有了  666
```



### 3. 引用

> 其他项目中使用

```js
npm i gocoms  
```

```js
// main.js
import 'gocoms/style.css'

// 使用组件
<template>
  <GoButton></GoButton>
  <GoInput></GoInput>
</template>
<script setup>
  import {GoButton,GoInput} from 'gocoms'
</script>
```


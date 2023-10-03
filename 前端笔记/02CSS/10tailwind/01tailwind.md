---
title: 01.tailwind
description: tailwind
published: 1
date: 2023-06-09T10:17:22.065Z
tags: tailwind
editor: markdown
dateCreated: 2023-04-30T18:24:14.303Z
---

<center>TailWind</center>



[toc]







## TailWind Css

> 功能类优先的 CSS 框架，用于快速构建定制的用户界面。[tw](https://www.tailwindcss.cn/) [en](https://tailwindcss.com/)





### 1. 安装

> vue3中

```css
# 安装依赖
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest

# 生成配置文件
npx tailwindcss init -p 

# 配置
module.exports = {
  content: ['./index.html', './src/**/*.{vue,js,ts,jsx,tsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

> 更目录创建`index.css`

```css
/*  styles.css */
@tailwind base; //重置默认属性
@tailwind components; //一些组件样式
@tailwind utilities; //工具类，也就是最常用的样式封装
```

```js
// main.js 引用
import './index.css'
```



### 2. 插件

> 提示插件

```shell
1. postcss 插件  PostCSS Language Support
2. 提示插件       Tailwind CSS IntelliSense
```



### 3.使用

> 用法: [buke](https://www.jianshu.com/p/3786bdf80b81)





### 4.生态

> tailwind的生态[宝藏](https://asmcn.icopy.site/) [tailwind](https://asmcn.icopy.site/awesome/awesome-tailwindcss/) [flowift](https://flowrift.com/c/blog)

> 复制需要的组件就可以了

```css
# tw.css(base) 提取样式

# base 基础
@layer base{
    h1{
        @apply font-mono text-cyan-200
    }
    h2{
        @apply font-light text-blue-950 bg-slate-100
    }
}

# components 组件封装
@layer components{

    .box_contern{
        @apply bg-white py-6 sm:py-8 lg:py-12
    }


    .btn{
        @apply px-4 py-2 rounded text-white hover:bg-slate-400 transition duration-100 ease-in-out
    }

    .btn-primary{
        @apply btn bg-green-400
    }
    .btn-denger{
        @apply btn bg-red-400
    }
}


# 功能类: twssc组件没有提供的功能
@layer utilities{
    .list-img-\@{
        list-style-image: url('aaa.png');
    }
}
```


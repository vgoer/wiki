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





#### 1.颜色 `类名= 使用目标+颜色+权重`

> 一般都把颜色作为背景色、文字颜色或者边框颜色。举个🌰，颜色green：

```css
文字颜色： text-green  
背景颜色： bg-green  
边框颜色1： border-green      //default，不需要数字描述  
边框颜色2： border-green-700  //数字表示颜色的深浅，越大颜色越深  
```



#### 2.文本 `"text-"`

```css
text-center
```

| Class        | Properties           |
| ------------ | -------------------- |
| text-left    | text-align: left;    |
| text-center  | text-align: center;  |
| text-right   | text-align: right;   |
| text-justify | text-align: justify; |
| text-start   | text-align: start;   |
| text-end     | text-align: end;     |

```css
text-decoration
```

| underline    | text-decoration-line: underline;    |
| ------------ | ----------------------------------- |
| overline     | text-decoration-line: overline;     |
| line-through | text-decoration-line: line-through; |
| no-underline | text-decoration-line: none;         |

```css
text-indent
```

| indent-0   | text-indent: 0px;                |
| ---------- | -------------------------------- |
| indent-px  | text-indent: 1px;                |
| indent-0.5 | text-indent: 0.125rem; /* 2px */ |
| indent-1   | text-indent: 0.25rem; /* 4px */  |



#### 3.字体 `"font-"`

> 字体类型 "font-"+{type}

| Class      | Properties                                                   |
| ---------- | ------------------------------------------------------------ |
| font-sans  | font-family: ui-sans-serif, system-ui, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji"; |
| font-serif | font-family: ui-serif, Georgia, Cambria, "Times New Roman", Times, serif; |
| font-mono  | font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace; |

> 字体粗细： "font-"+{weight}

| Class           | Properties        |
| --------------- | ----------------- |
| font-thin       | font-weight: 100; |
| font-extralight | font-weight: 200; |
| font-light      | font-weight: 300; |
| font-normal     | font-weight: 400; |
| font-medium     | font-weight: 500; |
| font-semibold   | font-weight: 600; |
| font-bold       | font-weight: 700; |
| font-extrabold  | font-weight: 800; |
| font-black      | font-weight: 900; |

> #### 行高 `"leading-"+{size}`

| Class     | Properties                       |
| --------- | -------------------------------- |
| leading-3 | line-height: .75rem; /* 12px */  |
| leading-4 | line-height: 1rem; /* 16px */    |
| leading-5 | line-height: 1.25rem; /* 20px */ |
| leading-6 | line-height: 1.5rem; /* 24px */  |
| leading-7 | line-height: 1.75rem; /* 28px */ |

```css
<h3 class="h-[100px] leading-[100px]">hello</h3>
```



#### 5.背景 `"bg-"`

```css
bg-purple-600 bg-opacity-100
```

| Class          | Properties                      | Preview |
| -------------- | ------------------------------- | ------- |
| bg-inherit     | background-color: inherit;      |         |
| bg-current     | background-color: currentColor; |         |
| bg-transparent | background-color: transparent;  |         |
| bg-black       | background-color: rgb(0 0 0);   |         |

```css
bg-repeat
```

| Class           | Properties                    |
| --------------- | ----------------------------- |
| bg-repeat       | background-repeat: repeat;    |
| bg-no-repeat    | background-repeat: no-repeat; |
| bg-repeat-x     | background-repeat: repeat-x;  |
| bg-repeat-y     | background-repeat: repeat-y;  |
| bg-repeat-round | background-repeat: round;     |
| bg-repeat-space | background-repeat: space;     |

```css
bg-[]
```

| Class             | Properties                                                   |
| ----------------- | ------------------------------------------------------------ |
| bg-none           | background-image: none;                                      |
| bg-gradient-to-t  | background-image: linear-gradient(to top, var(--tw-gradient-stops)); |
| bg-gradient-to-tr | background-image: linear-gradient(to top right, var(--tw-gradient-stops)); |
| bg-gradient-to-r  | background-image: linear-gradient(to right, var(--tw-gradient-stops)); |

```css
<div class="bg-gradient-to-l hover:bg-gradient-to-r  from-green-100 to-green-800">
  <!-- ... -->
</div>
```





#### 7. 边距 `"p-" "m-"`

> 内边距padding： 使用`p{t|r|b|l|x|y}-{size}`功能类控制元素一侧的内边距。
>
> 外边距margin： 使用 `m{t|r|b|l|x|y}-{size}`,用法同padding

| Class | Properties                             |
| ----- | -------------------------------------- |
| p-0   | padding: 0px;                          |
| px-0  | padding-left: 0px; padding-right: 0px; |
| py-0  | padding-top: 0px; padding-bottom: 0px; |
| ps-0  | padding-inline-start: 0px;             |
| pe-0  | padding-inline-end: 0px;               |
| pt-0  | padding-top: 0px;                      |
| pr-0  | padding-right: 0px;                    |



#### 8. 布局 display

> display：元素显示类型

| Class              | Properties                   |
| ------------------ | ---------------------------- |
| block              | display: block;              |
| inline-block       | display: inline-block;       |
| inline             | display: inline;             |
| flex               | display: flex;               |
| inline-flex        | display: inline-flex;        |
| table              | display: table;              |
| inline-table       | display: inline-table;       |
| table-caption      | display: table-caption;      |
| table-cell         | display: table-cell;         |
| table-column       | display: table-column;       |
| table-column-group | display: table-column-group; |
| table-footer-group | display: table-footer-group; |
| table-header-group | display: table-header-group; |
| table-row-group    | display: table-row-group;    |
| table-row          | display: table-row;          |
| flow-root          | display: flow-root;          |
| grid               | display: grid;               |
| inline-grid        | display: inline-grid;        |
| contents           | display: contents;           |
| list-item          | display: list-item;          |
| hidden             | display: none;               |

> Flex `"flex-"`

> `box-sizing ：`控制浏览器如何计算元素的总大小的功能类。

| Class       | Properties               |
| ----------- | ------------------------ |
| box-border  | box-sizing: border-box;  |
| box-content | box-sizing: content-box; |



#### 9. 伪类 `{ hover: | focus: | checked: |active: | visited: |disabled: } + 功能类`

> 并不是所有功能类都可以放在伪类的后面，只有tailwind文档规定的才可使用，如果需要在tailwind的配置文件中配置variants选项。

```html
//hover active
<button class="bg-red-500 hover:bg-red-700 active:bg-purple-500 ">
  Hover me
</button>
//disabled
<button class="disabled:opacity-50">
  Submit
</button>
//checked
<input type="checkbox" class="appearance-none checked:bg-blue-600">
```





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


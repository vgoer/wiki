---
title: 01less语法
description: 
published: 1
date: 2023-06-27T10:48:25.339Z
tags: 
editor: markdown
dateCreated: 2023-06-27T18:48:19.889Z
---

<center>less</center>



[toc]





## less

> css预处理器 更好的编辑修改维护代码 [less](https://github.com/less/less.js)
>
> `Easy LESS`：vscode插件自动把less编译为css





### 1. 安装

```shell
npm i -g less

lessc -v  # 安装成功 安装vscoed插件

# 手动编译
lessc demo.less demo.css
```





### 2. 使用

> less常用的用法 [blog](https://juejin.cn/post/7035590363418984455)



#### a.注释

> `less` 中使用`单行注释`和`多行注释`，其中但行注释不会被编译到 `CSS` 中去。

```less
// 普通注释,不会被编译到 CSS 中

/*
 多行注释,会被编译到 CSS 中
*/
```

#### b.变量

> 我们可以使用 `@` 符号开头定义变量，注意要在结尾处加上分号。

```less
@变量名：值;

@bgcolor:#008c8c;

body{
    background-color:@bgcolor;
}
```

> 实际开发中，根据不同的类型来命名不同的变量，这样不但便于理解，而且易于维护。

```less
/* ============ 距离 ============ */
@space-large: 36px;
@space-middle: 24px;
@space-small: 12px;
// ...

/* ============ 颜色 ============ */
// 颜色可以说是一个项目中定义最多的变量了,可以单独拆分出一个 less 文件进行维护
@color-theme: #0094ff;
@title-color: #090909;
```



#### c.转义

> 可以使用任何字符串，在使用时该字符串保持原样输出。

```less
@min-1024: ~"(min-width: 1024px)";

.box{
    @media @min-1024 {
        color: skyblue;
    }
}
```

### d. 嵌套

> 嵌套写法    **交集/伪类/伪元素选择器**用`&`

```less
@bgcolor: #008c8c;

.content{
    transition: all 1s ease;
    &:hover{
        background-color: @bgcolor;
        color: #fff;
        letter-spacing: 4px;
    }  
}
```



#### e. 运算

> less 运算   less提供了 **加 (+), 减 (-), 乘 (\*), 除 (/)** 算术运算.

**注意:**

1. 参与运算的两个数字, 可以一个有单位, 一个没有单位, 最终单位就是唯一的那个单位.
2. 参与运算的两个数字, 如果两个都有单位, 最终单位取第一个数字的单位.
3. **运算符左右必须加空格**, 否则有时候会出问题 (常见于加法).
4. **除乘法要加括号**, 否则不生效.



```less
// 根据媒体查询,设置不同屏幕中的html字号
// 屏幕划分为15等份
// 默认显示html字号参考750px的宽度
@no: 15;

html{
    font-size: 50px;
}
@media screen and (min-width:500px) {
    
    html{
        font-size: (500px / @no);
    }
}
@media screen and (min-width:750px) {
    html{
        font-size: (750px / @no);
    }
}
```



#### f.作用域

> 类似于 `JavaScript` 中的块级作用域  如果有两个同名变量，那么谁和使用行近谁生效。

```less
.dome {
    @color-title: pink;
    .item{
        @color-title:skyblue;
        .title {
            // skyblue
            color:@color-title;
        }
    }
}
```



#### g. 映射

> 以提高 `Less` 代码的可复用性。就像 `JavaScript` 中的 `Object` 的键值对一样，可以有多个，互不干扰。

```less
// before 
@color-theme:#008c8c;
@color-title: #ebf8ff;
@color-success: #29b5ff;

// after
#colors(){
    theme: #008c8c;
    title: #ebf8ff;
    success:#29b5ff;
}

.content{
    color: #colors[theme];

    .title{
        color: #colors[title];
    }
}

```



### h. 混合(mixin)

> 混合即 `Mixin` ，是一种复用代码的手段。可以将一个类名下的代码块轻易的放入另一个类名之中。

```less
// mix
// ()加了括号不会被编译了
.border-mixin(){
    border: 1px solid #008c8c;
}
h2{
    height: 100px;
    //  或者写成border-mixin;
    .border-mixin();
}

h3{
    .border-mixin();

}
```

> `extend` extend 是 Less 的一个伪类。它可继承 所匹配声明中的全部样式。

```less
// 继承样式，不用全部 全部 复制过去
.animation{
    transition: all .3s ease-out;
    .hide{
        transform:scale(0);
    }
}
#main{
    &:extend(.animation);
}
#con{
    &:extend(.animation .hide);
}
```



#### j.导入

> 引入其他文件 

```less
// 两种方式
@import url(./index.less);
@import './index.less';

.content{
    background-color: @bg-color;
}
```





#### k.循环

> [blog更多语法](https://juejin.cn/post/6844903520441729037) 模拟了生成栅格系统。

```less
.gen-columns(10);

.gen-columns(@n, @i: 1) when (@i <= @n){
    .columns-@{i}{
        width: (@i * 100% / @n);
    }
    .gen-columns(@n, (@i+1))
}
```



#### l.条件

> Less 没有 if else，可是它有 `when`

```less
#card{
    // and 运算符 ，相当于 与运算 &&，必须条件全部符合才会执行
    .border(@width,@color,@style) when (@width>100px) and(@color=#999){
        border:@style @color @width;
    }

    // not 运算符，相当于 非运算 !，条件为 不符合才会执行
    .background(@color) when not (@color>=#222){
        background:@color;
    }

    // , 逗号分隔符：相当于 或运算 ||，只要有一个符合条件就会执行
    .font(@size:20px) when (@size>50px) , (@size<100px){
        font-size: @size;
    }
}
#main{
    #card>.border(200px,#999,solid);
    #card .background(#111);
    #card > .font(40px);
}
```


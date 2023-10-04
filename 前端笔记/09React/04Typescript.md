---
title: 04.Typescript
description: Typescript
published: 1
date: 2023-06-09T10:15:14.305Z
tags: typescript
editor: markdown
dateCreated: 2023-05-05T12:28:15.633Z
---

<center>Typescript</center>



[toc]







## Typescript

> `Js` 网景公司推出的网页的脚本语言
> `Es` ==一种规范== 各个互联网公司推出自己的脚本语言。`ECMA欧洲计算机协会`
>
> `Ts` 微软推出的js的 超集 ==ts编译为js==





### 1. 安装

> ts[ts](https://www.tslang.cn/)

```shell
npm init -y 

# typescript 编译器
# ts-node 本地开发调试
# @types/node 类型定义
npm i -D typescript ts-node @types/node
```

```shell
# 全局
npm install -g typescript

# 初始化
tsc --init 

    "target": "es2016",   打包的js环境
    // "lib": [],         使用的库
    // "outDir": "./",    输出的目录
    "removeComments": true, 去注释
    "strict": true,        严格模式
```



### 2. ts开始

```js
let str: string = 'Hello worle'
console.log(str)
```

> `tsc`编译为js

```shell
# 编译
tsc

# 执行
node dist/main.js 
```



### 3. 强制类型

> 定义类型，不同类型不能赋值

```tsx
// 联合类型  可以是字符串和数字 any  boolean 等等
let name: string | number   = "vgoer"
name = 123
```

> 类型定义

```tsx
// 自定义类型  
type MyNameType = 'goer' | 'vgoer' | 'tgoer'

let s: MyNameType = 'goer'

// 数组
type MateDate = [string?,number?,boolean?]
let x:MateDate = []
x.push('xxxname')
x.push(123)
```

> 枚举：从0开始计算。 类似`go里面 iota` 常量计数器

```js
// 维护 比 type 更方便
enum JobState{
    new,     // 0   new = 10 // 10开始
    pending, // 1
    working,
    filed,
    abort
}

const x: JobState = JobState.abort // 4

console.log(x)
```

> 接口

```js
// 接口类型
interface Room{

    name: string,
    size: number,
    hase?: boolean

    setTemp: (tmp:number) => void
}

const room1: Room = {
    name: '1001',
    size: 100,
    setTemp: (x) => console.log(`this is ${x}`)
}

room1.setTemp(room1.size);
```

> 函数接口

```js
interface FunAdd{
    (a:number, b:number) : number;
}
const f: FunAdd = (a, b) => {

    return a + b
}

let sum = f(10,10)
console.log(sum)
```


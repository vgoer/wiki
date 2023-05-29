---
title: 05.Ts+React
description: Ts+React
published: 1
date: 2023-05-29T04:15:30.761Z
tags: react, typescript
editor: markdown
dateCreated: 2023-05-05T12:29:06.598Z
---

<center>Ts+React</center>





[toc]







## Ts+React

> ts语法+React





### 1. 创建

```shell
npx create-react-app my-ts-app --template typescript

npm i styled-components 

npm i --save-dev @types/styled-components
```

> 标准解构props里面的值

```tsx
interface ButtonProps{
    color:string;
}
// React.FC react组件 ButtonProps接口
const MyButton: React.FC<ButtonProps> = ({color}) => {

    return (
        <div>
            <button style={{color}}>add</button>
        </div>
    )
}
```



### 2.继承

> 继承原来 的html属性

```tsx
interface ButtonProps
// 接口继承原来的 html button的属性了
extends React.ButtonHTMLAttributes<HTMLButtonElement>
{
    color:string;
}
const MyButton: React.FC<ButtonProps> = (props) => {

    return (
        <div>
            <button style={{color:props.color}} {...props}>add</button>
        </div>
    )
}

<MyButton color='red' disabled></MyButton>
```

> 比如我们input标签有 `placeholder`属性要如何加呢

```tsx
interface InputPorps
extends React.InputHTMLAttributes<HTMLInputElement>
{
    color: string
}
const MyInput: React.FC<InputPorps> = (props) => {
    return (

        <div>
            <input type="text" {...props} style={{border:`1px solid ${props.color}`}}/>
        </div>
    )
}

 <MyInput placeholder='please...'  color="blue"></MyInput>
```



### 组件是传参

> 参数 就是一个组件呢

```tsx
const IconA = () => (
    <span>**</span>
)

interface ButtonProps
// 接口继承原来的 html button的属性了
extends React.ButtonHTMLAttributes<HTMLButtonElement>
{
    color:string;
    icon?: React.ReactNode
}
const MyButton: React.FC<ButtonProps> = (props) => {

    return (
        <div>
            <button style={{color:props.color}} {...props}>add {props.icon} </button>
        </div>
    )
}

<MyButton color='red' icon={IconA()} ></MyButton>

-- 添加

{
    color:string;
    icon?: React.ReactElement<any>
}
<MyButton color='red' icon={<IconA />} ></MyButton>
    
// React.ReactNode  ->  icon={IconA()}
//  React.ReactElement<any> -> icon={<IconA />}
```












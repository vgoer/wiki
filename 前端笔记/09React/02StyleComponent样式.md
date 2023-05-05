---
title: 02.StyleComponent样式
description: StyleComponent样式
published: 1
date: 2023-05-05T11:51:48.946Z
tags: react
editor: markdown
dateCreated: 2023-05-03T13:00:04.941Z
---

<center>StyleComponent</center>



[toc]





## StyleComponent

> `StyleComponent` react样式组件库 [style](https://styled-components.com/)
>
> React打通了`js+html` 但样式问题不好解决
>
> `StyleComponent`打通`react+css`





### 1.安装

```shell
npm i styled-components

# 安装插件
vscode-styled-components
```





### 2. 使用

> `styled.a` styled加标签名
>
> `&.red` 加类名

```jsx
import styled from 'styled-components'
// 添加一个组件
const MyButton = styled.a`
    // 写样式了
    border:1px solid #0ff;
    border-radius: 4px;
    padding: 14px;

    // & 代表自己
    & span{
        color:red;
    }

    &:hover{
        background-color:blue;
    }

    // css的选择器都可以用
    & + & {
        margin-top:20px;
    }

    // 加类名
    &.red{
        border:3px solid #f00;
    }
`
function App() {
  return (
    <div className="App">
        <div className='box'>
            <MyButton>Add <span>!!!</span> </MyButton>
            <MyButton className='red'>Add <span>!!!</span> </MyButton>
        </div>
    </div>
  );
}
```



### 2. 传值

> `props` 传值到样式里

```jsx
height: ${ props => props.size || 'auto'};

<MyButton size="100px">Add <span>!!!</span> </MyButton>
```



### 3. 设置样式

> `css` 选中设置样式

```jsx
import styled,{css} from 'styled-components'
<MyButton disabled={ true }>Add</MyButton>

// 设置css样式
const MyButton = styled.a`
    // 写样式了
    border:1px solid #0ff;
    border-radius: 4px;
    padding: 14px;
    height: ${ props => props.size || 'auto'};

    // 设置css styled-components里面的
   ${props => props.disabled && css`
        color:blue;
        border:1px solid blue;
        text-align:center;
   `}
`
```



### 4. 继承

> mixin概念： 继承样式 面向对象

```jsx

import styled,{css} from 'styled-components'
// 添加一个组件
const MyButton = styled.a`
    // 写样式了
    border:1px solid #0ff;
    border-radius: 4px;
    padding: 14px;
    height: ${ props => props.size || 'auto'};


    // 设置css样式
   ${props => props.disabled && css`
        color:blue;
        border:1px solid blue;
        text-align:center;
   `}
`

// 继承父类样式
const BigButton = styled(MyButton)`
    height:100px;
    line-height:100px;
    color: #fff;
    background-color:blue;
`
function App() {
  return (
    <div className="App">
        <div className='box'>
            <MyButton disabled={ true }>Add</MyButton>

            <BigButton>Chage</BigButton>
        </div>
    </div>
  );
}
```



### 5. 全局样式

> 设置全局样式

```jsx
touch global.style.tsx

import {createGlobalStyle} from 'styled-components'

// 全局样式
export const GlobalStyle = createGlobalStyle`
    
    ×{
        margin:0;
        padding:0;
    }

    .debug-r{
        outline: 2px solid #f00;
    }
`
```

> 引入使用 `index.js`

```jsx
import {GlobalStyle} from './global.style.tsx'

<GlobalStyle/>
<App />

// 全局使用了
<BigButton className='debug-r'>Chage</BigButton>
```



### 6.动画

> 添加动效

```jsx
import styled,{css,keyframes} from 'styled-components'
// 添加一个组件


// 定义动画
const rotate = keyframes`
    from{
        transform:rotate(0deg);
    }
    to{
        transform:rotate(360deg)
    }

`
const Rotate = styled.div`
    width:100px;
    height:100px;
    border-radius:50%;
    text-align:center;
    line-height:100px;
    border:1px solid ${props => props.color || 'blue'};
    animation: ${rotate} 1s linear infinite;

`

function App() {
  return (
    <div className="App">
        <div className='box'>
            <Rotate color='blue'>动画</Rotate>
        </div>
    </div>
  );
}
```


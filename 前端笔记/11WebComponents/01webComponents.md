---
title: 01webComponents
description: 
published: 1
date: 2023-07-05T13:06:18.869Z
tags: 
editor: markdown
dateCreated: 2023-07-05T13:06:17.371Z
---

<center>Web Components</center>



[toc]







## Web components

> web components: 解决传统前端痛点







### 1. 传统前端

> 传统前端(html+css+js):  ==重复== , ==冲突==

> 解决思路

```js
组件化： 封装 + 复用

框架： react 、  vue 、 angular 缺点：size很大  开发调试：时间长
原生:  web components
```





### 2. 最重要的三部分 ( 0 >>> 1)

>- template
>
>- shadow
>
>- custom elements

```js
1. // 创建项目
npm init 

2. // BrowserSync是一个强大的工具，可以帮助开发人员在浏览器中实时预览和同步更改。
npm install -g browser-sync

3. // 使用
browser-sync start --server --files "*.*"

4. // package.json添加启动命令
 "start": "browser-sync start --server --files \"*.*\""
```

> index.html

```html
<body>
    <h1>hello</h1>

    <wc-card class="box"></wc-card>
    <script src="./wc.js"></script>
</body>
```

```js
// wc.js
/**
 * 定义样式和标签
 * @returns 字符串
 */
const cardHtml = () => `
    <style>
    div{
        padding: 8em;
        border: 1px solid blue;
        border-radius: 10px;
        box-shadow: 2px 2px 10px 2px blue inset;
    }
    h1{
        text-align: center;
        text-shadow: 2px 2px blue;
    }
    button{
        padding: 10px;
        width: 100px;
        border: none;
        cursor: pointer;
        color: #008c8c;
        transition: all 1s ease;
    }
    button:hover{
        box-shadow: 3px 3px 2px 2px blue;
    }
    </style>
    <div>
        <h1>hello</h1>
        <button>add</button>
    </div>
`;

// 创建 template
const template = document.createElement('template')

template.innerHTML = cardHtml()


class Card extends HTMLElement{
    
    constructor(){
        super();

        // shadow 概念 重要 （自己的命令空间等)
        this.attachShadow({mode:'open'})
        this.shadowRoot.appendChild(template.content.cloneNode(true))
    }
}
// 定义一个自定义标签
window.customElements.define('wc-card',Card)

```

> 另一种写法 可以传值 (不太常用)

```js
/**
 * 定义样式和标签
 * @returns 字符串
 */
const cardHtml = (props) => `
    
    <style>
    div{
        padding: 8em;
        border: 1px solid ${props.color};
        border-radius: 10px;
        box-shadow: 2px 2px 10px 2px ${props.color} inset;
    }
    h1{
        text-align: center;
        text-shadow: 2px 2px ${props.color};
    }
    button{
        padding: 10px;
        width: 100px;
        border: none;
        cursor: pointer;
        color: ${props.color};
        transition: all 1s ease;
    }
    button:hover{
        box-shadow: 3px 3px 2px 2px ${props.color};
    }
    </style>
    <div>
        <h1>${props.title}</h1>
        <button>add</button>
    </div>
`;


class Card extends HTMLElement{
    
    constructor(){
        super();

        this.title = "goer"
        this.color = "#008c8c"
        // shadow 概念 重要 （自己的命令空间等)
        this.attachShadow({mode:'open'})
        // 传递参数过去
        this.shadowRoot.innerHTML = cardHtml(this)
    }
}
// 定义一个自定义标签
window.customElements.define('wc-card',Card)

```





### 3. 其他概念 ( 1 >>> n)

> - slot： vue插槽(子组件) react（children组件）
>
> - lifecycle 生命周期
> - event 事件注册
> - state 状态
> - attribute 设置属性值

> 子组件

```js

    <wc-card class="box">
        <div slot="child">我是子组件</div>
    </wc-card>
    <wc-card class="box"></wc-card>

// js
/**
 * 定义样式和标签
 * @returns 字符串
 */
const cardHtml = (props) => `
    
    <style>
    div{
        padding: 8em;
        border: 1px solid ${props.color};
        border-radius: 10px;
        box-shadow: 2px 2px 10px 2px ${props.color} inset;
    }
    h1{
        text-align: center;
        text-shadow: 2px 2px ${props.color};
    }
    button{
        padding: 10px;
        width: 100px;
        border: none;
        cursor: pointer;
        color: ${props.color};
        transition: all 1s ease;
    }
	// 样式
    slot[name="child"]{
        color:red;
    }

    button:hover{
        box-shadow: 3px 3px 2px 2px ${props.color};
    }
    </style>
    <div>
        <h1>${props.title}</h1>
        
        // 子组件插槽
        <slot name="child"></slot>
        
        <button>add</button>
    </div>
`;
```

> lifecycle 生命周期
>
> event  事件注册

```js

class Card extends HTMLElement{
    
    constructor(){
        super();

        // shadow 概念 重要 （自己的命令空间等)
        this.attachShadow({mode:'open'})
        this.shadowRoot.appendChild(template.content.cloneNode(true))
    }
    // 事件
    myOnClick(){
        alert('click')
    }

    // lifecycle 
    // 组件创建之后
    connectedCallback(){
        // 注册一个点击事件
        this.shadowRoot.querySelector('button').addEventListener('click',()=>this.myOnClick())
    }
    // 组件消失移除
    disconnectedCallback(){
        this.shadowRoot.querySelector('button').removeEventListener()
    }
    // attribute 发生变化时候
    attributeChangedCallback(propeName,oldValue,newValeu){
        alert(propeName,oldValue,newValeu)
    }
}
```

> state 状态

```js
// wc.js
// wc.js
/**
 * 定义样式和标签
 * @returns 字符串
 */
const cardHtml = () => `
    <style>
    div{
        padding: 8em;
        border: 1px solid blue;
        border-radius: 10px;
        box-shadow: 2px 2px 10px 2px blue inset;
    }
    h1{
        text-align: center;
        text-shadow: 2px 2px blue;
    }
    button{
        padding: 10px;
        width: 100px;
        border: none;
        cursor: pointer;
        color: #008c8c;
        transition: all 1s ease;
    }
    button:hover{
        box-shadow: 3px 3px 2px 2px blue;
    }
    </style>
    <div>
        <h1>hello</h1>
        
        <p class="flag"></p>

        <button>add</button>
    </div>
`;

// 创建 template
const template = document.createElement('template')

template.innerHTML = cardHtml()

class Card extends HTMLElement{
    
    constructor(){
        super();

        // state
        this.flag = 0

        // shadow 概念 重要 （自己的命令空间等)
        this.attachShadow({mode:'open'})
        this.shadowRoot.appendChild(template.content.cloneNode(true))
    }
    // 事件
    myOnClick(){

        this.flag++;
        const flagEle = this.shadowRoot.querySelector('.flag')
        // flagEle.innerHTML = this.flag
        flagEle.innerText = this.flag.toString()
    }

    // lifecycle 
    // 组件创建之后
    connectedCallback(){
        // 注册一个点击事件
        this.shadowRoot.querySelector('button').addEventListener('click',()=>this.myOnClick())
    }
}
// 定义一个自定义标签
window.customElements.define('wc-card',Card)
```

> attribute 设置属性值

```js
    <wc-card class="box" title="One">
        <div slot="child">我是子组件</div>
    </wc-card>
    <wc-card class="box" title="Tow"></wc-card>

// wc.js

class Card extends HTMLElement{
    
    constructor(){
        super();

        // state
        this.flag = 0

        // shadow 概念 重要 （自己的命令空间等)
        this.attachShadow({mode:'open'})
        this.shadowRoot.appendChild(template.content.cloneNode(true))


        // attribute
        this.shadowRoot.querySelector('h1').innerHTML = this.getAttribute('title')
    }
}
```





> web components的框架： Lit , Polymer 等










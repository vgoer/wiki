---
title: 02lit
description: 
published: 1
date: 2023-07-05T13:06:21.044Z
tags: 
editor: markdown
dateCreated: 2023-07-05T13:06:19.593Z
---

<center>lit</center>



[toc]







## lit

> Lit 是一个依据 Web-Component 构建的前端结构 [lit](https://github.com/lit/lit)
>
> - 依据 Web-Component 的更高层封装，供给了现代前端开发习气的照顾式数据，声明式的模版，减少了web component的一部分样板代码.
> - 小。作业时仅有5K
> - 功用强悍。规避了 VDOM 的一些害处，更新时仅处理 UI 中的异步部分（能够了解成仅处理照料式的部分）
> - 兼容性较好。由于 web-component 是 HTML 的原生才调，也就代表着 web-component 能够在任何运用 HTML 的当地运用，结构无关
> - 谷歌开发，基于Web Component，可以用于构建共享组件，可以用在vue中，也可以用在其他框架中



### 1. 安装使用

> blog : [csdn](https://blog.csdn.net/weixin_41897680/article/details/125144720)

```shell
npm i lit
```

> ts

```js
import {LitElement, css, html} from 'lit';
import {customElement, property} from 'lit/decorators.js';

@customElement('simple-greeting')
export class SimpleGreeting extends LitElement {
    static styles = css`
        :host {
        color: blue;
        }
    `;

    @property()
    name?: string = 'World';

    render() {
        return html`<p>Hello, ${this.name}!</p>`;
    }
}
```


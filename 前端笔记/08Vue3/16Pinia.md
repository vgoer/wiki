---
title: 16.Pinia
description: Pinia
published: 1
date: 2023-06-09T10:14:41.621Z
tags: vue3
editor: markdown
dateCreated: 2023-04-24T12:15:03.868Z
---

<center>Pinia</center>





[toc]





## Pinia

> vue3的状态管理库

```js
// 优点  与vuex
1. 符合vue3的组合api
2. 更支持typescript 
```



### 1. 安装

```shell
npm install pinia  
```



### 2. 引入

```tsx
// 引入和挂载  main.ts
import {createPinia} from 'pinia'
const pinia = createPinia()

createApp(App).use(pinia).mount('#app')
```



### 3. 状态仓库的事

```tsx
// 1. 定义状态容器和数据
// 2. 修改容器中的state
// 3. 仓库中的 action的使用

import {defineStore} from 'pinia'
// 暴露接口
export const mainStore = defineStore('main',{
    // 状态数据
    state:()=>{
        return{
            sayMsg:'Hello Vue3'
        }
    },
    // 监视状态
    getters:{},
    // 
    actions:{

    },
})
```

```vue
# 绑定到页面
<script setup lang="ts">
import { mainStore } from './store/index';
const store = mainStore();
console.log(store);
</script>

<template>
  <h3>{{store.sayMsg}}</h3>
  <Menu></Menu>
</template>
```



### 4. 修改状态数据

#### a.直接修改

> 修改 store 数据 并解构出来

```vue
<template>
    <button @click="setDate">修改状态数据</button>
</template>

<script setup lang="ts">
 
    import { mainStore } from '../store/index'
	
    // 修改全局的状态数据
    const store = mainStore()
    const setDate = () => {
        store.cont++
    }
</script>
```

> 解构为响应式的数据`

```js
// storeToRefs 方法
import { mainStore } from './store/index';
import {storeToRefs} from 'pinia'
const store = mainStore();

// storeToRefs 将store数据变为响应式的
const {sayMsg,cont} = storeToRefs(store);

console.log(store);
```

#### b.多个一起修改

> `$patch` 一起修改多个

```vue
<template>
    <button @click="setDate">修改状态数据</button>
    <button @click="setMoreDate">修改状态数据</button>
</template>

<script setup lang="ts">
 
    import { mainStore } from '../store/index'

    const store = mainStore()
    const setDate = () => {
        store.cont++
    }
	// 同时修改多个状态数据
    const setMoreDate = () => {
        store.$patch({
            cont: store.cont+2,
            sayMsg: store.sayMsg === 'hi Vue3' ? 'vue3': 'hi Vue3',
        })
    }
</script>
```

### c.更复杂的应用

> 在`store 下的 actions`添加修改方法

```js
actions:{
    SetSatte(){
        this.sayMsg="vue pinia 修改",
            this.cont += 10
    }
},
    
// 使用
<button @click="store.SetSatte">修改数据</button>
```



### 5. Getters

> 类似vue中的计算属性，getters 自动缓存属性  ==常用==

```js
// 隐藏电话中间四位数字
// 状态数据
    state:()=>{
        return{
            sayMsg:'Hello Vue3',
            cont:0,
            phone:'13401009991'
        }
    },
    // 监视状态
    getters:{
        setPhone(state){
            console.log(state.phone);
            //   替换中间四个字符
            return state.phone.toString().replace(/^(\d{3})\d{4}(\d{4})$/,'$1****$2')
        }
    },
```



### 仓库间调用

```js
// 和上面类似 直接引入调用就ok了
// test 仓库
import { defineStore } from "pinia";


export const testStore = defineStore('test',{
    state(){
        return {
            objNmae: {
                name:'小白',
                age:20,
                love:'篮球',
            }
        }   
    }
})
```

```js
// main中使用
// 引入test仓库
import {testStore} from '../store/test'
// 状态数据
state:()=>{
    return{
        otherObj:testStore().objNmae,
    }
},
```

> 安装google插件调试 `vue-devtools`










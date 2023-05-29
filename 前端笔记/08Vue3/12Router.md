---
title: 11.Router
description: Router
published: 1
date: 2023-05-29T04:15:04.615Z
tags: vue3
editor: markdown
dateCreated: 2023-04-24T12:06:42.980Z
---

<center>Router</center>





[toc]





## Router

> vue.js 的路由，页面间的跳转  
>
> 博主：[小满zs](https://blog.csdn.net/qq1195566313/category_11696205.html?spm=1001.2014.3001.5482)



### 1. 安装

```shell
npm install vue-router@4
```



### 2.使用

> 在 src目录下新建`router目录` 新建 `index.ts`

```js
import {createRouter,createWebHashHistory,RouteRecordRaw} from 'vue-router'
// ts 的类型
const routes:Array<RouteRecordRaw> =  [
    // 首页
    {
        path:'/login',
        component:() => import("../components/Login.vue")
    },
    {
        path:'/reg',
        component:() => import("../components/Reg.vue"),
    }
    
]
export const router = createRouter({
    history:createWebHashHistory(),
    routes
})
```

> 挂载  `main.ts`

```js
import {router} from './router/index'

createApp(App).use(router).use(pinia).mount('#app')
```

> 渲染

```vue
1. router-link  to=""  跳转的路由 相当于a标签
2. router-view    路由匹配到的组件将渲染在这里
<template>
    <router-link to="/login">登录</router-link>
    <router-link to="/reg">注册</router-link>
    <router-view></router-view>
</template>
```



### 3.路由模式

> vue2: mod    vue3:history 

```js
//vue2 mode history vue3 createWebHistory        url 带 # 
//vue2 mode  hash  vue3  createWebHashHistory
//vue2 mode abstact vue3  createMemoryHistory

createWebHashHistory  ==> location.hash  带 # 
createWebHistory      ==> histroy        不带 #
```



### 4.命名路由

> 路由提供 `name`

```js
    {
        path:'/login',
        name:'login',
        component:() => import("../components/Login.vue")
    },
    {
        path:'/reg',
        name:'reg',
        component:() => import("../components/Reg.vue"),
    }

// 引入到组件中 以对象的方式
<router-link :to="{name:'login'}">登录</router-link>
<router-link :to="{name:'reg'}">注册</router-link>
<router-view></router-view>
```

> 编程式导航 

```vue
<template>
    <div> 
      <button @click="toPage('/login')">登录</button>
      <button @click="toPage('/reg')">注册</button>
    </div>

    <router-view></router-view>
</template>

<script setup lang="ts">
// 引入router
import { useRouter } from 'vue-router';
const router = useRouter()

const toPage = (url:string) => {
  // 编程式 路由
  router.push(url)
}
</script>
```

> 对象的形式 ==后面容易传递参数==

```js
const toPage = (url:string) => {
  // 编程式 路由
  router.push({
	path:url, // name:url
      
  })
}
```



### 5. 历史记录

> 路由访问的历史记录

1. 清除历史记录

```js
// 1. router-link 加 replace
<router-link replace :to="{name:'login'}">登录</router-link>

// 2. roter.replace()方法
// 编程式 路由
router.replace(url)
```

2. 前进和后退

```js
router.go(2)
router.back(3)
```



### 6.路由传参

> 传递参数

```js
https://xiaoman.blog.csdn.net/article/details/123613595
```







### 10.路由过渡动效

> 路由过渡动效

```js
// 安装 animate css 
npm install animate.css --save

// 路由定义mate元素
    {
        path:'/reg',
        name:'reg',
        meta:{
            transition:'animate__fadeInDown',
        },
        component:() => import("../components/Reg.vue"),
    }
```

> 引入和使用

```vue
//  v-slot=#default  API： 
<router-view #default="{route,Component}">
    <transition  :enter-active-class="`animate__animated ${route.meta.transition}`">
        <component :is="Component"></component>
    </transition>
</router-view>

<script>
// 引入css
import 'animate.css';
</script>
// 然后就可以切换了 牛牛牛
```




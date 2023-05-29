---
title: 14.vuex
description: vuex
published: 1
date: 2023-05-29T04:15:09.013Z
tags: vue3
editor: markdown
dateCreated: 2023-04-24T12:10:15.417Z
---

<center>Vuex</center>



[toc]



## 1. vuex

> 全局状态管理库（所有页面数据都改变） [vuex](https://vuex.vuejs.org/zh/index.html)





### 2. 安装使用

```shell
npm install vuex@next --save
```

```js
// store/index.js
import { createStore } from "vuex";


export default createStore({
    // 全局状态管理初始值
    state: {
        num: 100,
    },
    //计算state
    getters: {
		
    },
    //更新状态方法
    mutations: {
        editNum(state, num) {
            state.num = num
        }
    },
    // 可以异步操作 值还是交给  mutations修改
    actions: {
        setConetPro(content, num) {
            return new Promise((resolve, reject) => {
                if (num > 100) {
                    reject('值太大了')
                } else {
                    content.commit('editNum', num)
                    resolve()
                }
            })
        }
    },

    // 数据较多，模块化
    modules: {

    }
})
```

```js
// 引入和使用
<template>
    <h2>联系我们</h2>
    <p>{{ num }}</p>
    <button @click="chage">chage</button>
</template>
<script setup>
import { ref } from 'vue';
import { useStore } from 'vuex'

const myStore = useStore();
const num = ref(myStore.state.num)

const chage = () => {
    // commit(方法名,polyd) 
    // myStore.commit('editNum', 1);
	// actions里的方法
    myStore.dispatch('setConetPro', 101).then(res => {
        alert("修改成功")
    }).catch(err => {
        alert(err)
    })

    console.log(myStore.state.num)
}
</script>
<style scoped></style>
```

> 自定义userinfo-x

```js
export default {
    state: {
        userinfo: {}
    },
    getters: {

    },
    mutations: {
        setUserInfo(state, uinfo) {
            state.userinfo = uinfo
        }
    }
}
// vue-x index.js

import myUser from "./user/userinfo.js";
    // 数据较多，模块化
    modules: {
        myUser,
    }
```



> 路由守卫

```js
// 路由守卫
router.beforeEach((to, from, next) => {
    // 判段用户是否登录
    // 获取到状态管理仓库的userinfo
    console.log(store.state.myUser)

    const userinfo = store.state.myUser.userinfo;
    console.log(userinfo.name)
    if (!userinfo.name) {
        if (to.path === '/login') {
            next()
            return
        }
        next('/login')
    } else {
        next()
    }
})
```

```js
// 登录页
const store = useStore();
const router = useRouter();

const addLogin = () => {
    // console.log(formLabelAlign)
    // 使用userinfo 里面的状管理
    store.commit('setUserInfo', formLabelAlign)
    router.push({
        path: '/',
    })
}
```

> 登录成功，本地存储

```js
// 设置本地存储
localStorage.setItem('userInfo', JSON.stringify(formLabelAlign))

// 本地存储 写入到 vuex
state: {
    userinfo: (localStorage.getItem('userInfo') && JSON.parse(localStorage.getItem('userInfo'))) || {}
},

```

> 退出

```js
import { useRouter } from 'vue-router';
import { useStore } from 'vuex';

let store = useStore();
let router = useRouter()
const logOut = () => {
    localStorage.removeItem('userinfo')
    store.commit('setUserInfo', {});
    router.push({
        path: '/login',
    })
}
```



> axiox 封装

```shell
# request/service.js
import axios from "axios"


// 创建实例
const Service = axios.create({
    timeout: 8000,
    baseURL: "https://axxx.com",
    headers: {
        "Content-Type": "application/json;charset=utf-8",
    }
})

import { ElLoading } from 'element-plus'
const loading = null
//请求拦截器 loding效果
Service.interceptors.request.use(config => {
    loading = ElLoading.service({
        lock: true,
        text: 'Loading',
        background: 'rgba(0, 0, 0, 0.7)',
    })

    return config
})

import { ElMessage } from 'element-plus'
// 相应拦截
service.interceptors.response.use(response => {

    loading.close()
    return response.data
}, error => {
    ElMessage({
        message: '错误',
        grouping: true,
        type: 'error',
    })
    loading.close()
})
```


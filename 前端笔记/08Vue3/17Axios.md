---
title: 17.Axios
description: Axios
published: 1
date: 2023-04-29T17:47:20.743Z
tags: vue3
editor: markdown
dateCreated: 2023-04-24T12:16:08.240Z
---

<center>Axios</center>





[toc]



## Axios

> Axios是一个基于 *[promise](https://javascript.info/promise-basics)* 网络请求库  官网：[axios](https://www.axios-http.cn/) 

 

### 1. 安装



```shell
npm install axios 
```



### 2. 请求

> 发送请求

> 设置跨域(方法有很多)

```js
//  php 放到请求头
// 允许跨域
header('Access-Control-Allow-Origin: *');
header("Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept");
header('Access-Control-Allow-Methods: GET,POST');
```

```vue
# 请求
<template>
    <h3>Axiso 学习</h3>

    <div class="btn">
        <button @click="getDate">Get</button>
        <button @click="postDate">Post</button>
    </div>
</template>
<script setup lang="ts">

    import axios from 'axios'
    const getDate = () => {
        // get 接口
        axios.get('http://tp5code.com/home')
        .then((res) => {
            console.log(res)
        })
        .catch((err) => {
            console.log(err);
        })
    }
    const postDate = () => {
        // post 接口和数据
        axios.post('http://tp5code.com/home',{
            name:'goer',
            pass:'goerpd'
        })
        .then((res) => {
            console.log(res)
        })
        .catch((err) => {
            console.log(err);
        })
    }
</script>
```

```php
public function index()
{   
    // 后端测试请求  post()  get()
    $request = request()->post();
    if($request){
        re_arr(1,'yes',$request);
    }else{
        echo '000';
    }
}
```



### 3. 封装

> 重复太多了，进行封装

```js
// 新建 request 目录
// request.js 文件 做拦截
// 拦截文件

import axios from 'axios'

// 创建一个实例  官方文档
const instance = axios.create({
    // 最小url
    baseURL: 'https://some-domain.com/api/',
    // 超时
    timeout: 1000,
    // 请求头部
    headers: {'X-Custom-Header': 'foobar'}
  });


// 拦截器  -- 请求拦截


instance.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么  有些接口需要带 token
    let token = localStorage.getItem('token')
    if(token){
        // 设置头部带有token
        config.headers = {
            'token': token
        }
    }
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });


// 拦截器  -- 相应拦截
instance.interceptors.response.use(function (config) {
    // 在发送请求之前做些什么
return config;
}, function (error) {
// 对请求错误做些什么
return Promise.reject(error);
});


// 暴露接口
export default instance;
```

> api.js 接口目录

```js
// 引入拦截器

import request from './request'


// 按需导入需要的api

// 1. 请求get数据
export const GetApi = () => request.get('/home');  // api接口

// 这样些好像更简介明了
export const Getapifox = (query) => {
    return request({
        path:'/user/login',
        method:'post',
        params:query
    })
}

// 2. 请求post数据
export const PostApi = (params) => request.get('/home',params);  // api接口
```

```js
// 按需使用
// 按需引入接口
import {GetApi} from './request/api'    
const getDate = () => {
    GetApi()
        .then((res) => {
        console.log(res)
    })
        .catch((err) => {
        console.log(err);
    })
}

// 更简便
// 按需引入接口
import {GetApi } from '../Api/api'
const getDate = async () => {
    let res = await GetApi()
    console.log(res)
}
```








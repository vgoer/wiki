---
title: 07.Axios
description: axios
published: 1
date: 2023-06-09T10:15:22.984Z
tags: react
editor: markdown
dateCreated: 2023-05-05T17:23:17.768Z
---

<center>Axios</center>



[toc]







## Axios

> axios加React  其实和vue的配置一模一样 ==复习一边==





### 1.安装

```shell
npm i axios
```





### 2. 添加环境变量文件

```shell
.env  # 开发环境

NAME=app

REACT_APP_API_PATH=127.0.0.1
```

```shell
.env.production # 生产环境

name=proname
# 打包不生成map file
GENERATE_SOURCEMAP=false

REACT_APP_API_PATH=https://aa.com/api
```



### 3. 请求器

> 其实和vue一样，没法讲了

```js
// api/request.js

import axios from "axios"

const request = axios.create({
    baseURL:process.env.REACT_APP_API_PATH,
    timeout:2000,
    headers:{

    }
})
// 修改.env文件需重启
// console.log(process.env)
request.interceptors.request.use((config) => {

    return config
},(error) => {
    return Promise.reject(error)
})
request.interceptors.response.use((response) => {

    return response
},(error) => {

    return Promise.reject(error)
})

export default request
```

```js
// api/login.js

import request from "./request"

export const loginApi = (data) => {
    return request({
        url:'/admin/login',
        method:'post',
        data:data
    })
}

export const resgistApi = (data) => {
    return request({
        url:'/admin/regist',
        method:'post',
        data:data
    })
}
```


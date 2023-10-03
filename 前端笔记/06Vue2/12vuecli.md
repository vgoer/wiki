---
title: 12.vuecli
description: vuecli
published: 1
date: 2023-06-09T10:13:55.837Z
tags: vue2
editor: markdown
dateCreated: 2023-04-24T10:47:47.119Z
---

<center>vue-cli</center>

[toc]



### vue-cli

> vue-cli是一个基于 Vuejs 进行快速开发的完整系统，俗称vue全家桶，是使用Vuejs框架开发项目的利器。



vue-cli有三个组件

```
1、CLI：@vue/cli 全局安装的 npm 包，提供了终端里的vue命令（如：vue create 、vue serve 、vue ui 等命令）

2、CLI 服务：@vue/cli-service是一个开发环境依赖。构建于 webpack 和 webpack-dev-server 之上（提供 如：serve、build 和 inspect 命令）

3、CLI 插件：给Vue 项目提供可选功能的 npm 包 （如： Babel/TypeScript 转译、ESLint 集成、unit和 e2e测试 等）
```



#### 搭建

> 1. 安装nodejs   `node -v `查看版本，如果没安装，看node这章
> 2. 安装webpack包管理工具，webpack是vue-cli构建工具
>
> > `npm install webpack -g`

> vue -V  //查看版本

```shell
vue2                                vue3
npm install vue-cli -g;            npm install @vue/cli -g;
vue init webpack 项目名称           vue create 项目名称
            npm install   //安装依赖
npm run dev                        npm run server //开发者模式
```

> 安装vue-cli3.0,本地已经全局安装了vue-cli2.0的情况下,  先卸载vue.2



```
// 如果全局vue了   就开始创建项目了    一路回车就行
vue init webpack 项目名称         vue create 项目名称
npm run dev                     npm run server
npm run build    //编译命令
```

项目结构

```js
build    //构建脚本目录
config   //配置目录
node_modules   //node依赖工具包目录
src      //源码
	assets  //项目的js文件，图片等资源
	components  //组件目录
	router   //路由文件
	App.vue  //入口组件
	main.js  //入口组件的js文件
static    //静态资源目录
```



```
npm i vue-router --save   //當前目錄安裝路由
npm i axios --save   //异步的通信插件
npm i qs --save  //数据转换
npm i element-ui --save  //UI组件
```








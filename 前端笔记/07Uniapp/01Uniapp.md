---
title: 01.Uniapp
description: Uniapp
published: 1
date: 2023-04-24T10:52:15.213Z
tags: uniapp
editor: markdown
dateCreated: 2023-04-24T10:52:11.797Z
---

<center>uapp</center>



[toc]

## uin app

> 是一个使用 [Vue.js](https://vuejs.org/) 开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、Web（响应式）、以及各种小程序（微信/支付宝/百度/头条/QQ/快手/钉钉/淘宝）、快应用等多个平台。



#### 环境

> HBuilderX：[官方IDE下载地址](https://www.dcloud.io/hbuilderx.html) HBuilderX是通用的前端开发工具，但为uni-app做了特别强化。



1. vue手架

```shell
npm install -g @vue/cli
```

2. 新建app-vue

```shell
vue create -p dcloudio/uni-preset-vue 项目名称
```



### 开始

```shell
npm run serve // 开启app   
```

```shell
// 编译微信小程序  package.json 下的scripts
npm run dev:mp-weixin     //运行
npm run build:mp-weixin   //编译
```

> dist 编译文件  src 源码

1. 运行到浏览器，就可以看到项目了
2. 运行到手机模拟器

```tex
插上数据线连接手机
打开开发者模式 -- 手机版本不同，所以百度一下
开启usb调试模式 -- 自动要下载一个hy编辑器
```

3. 运行到电脑的其他小程序开发工具

```
设置 --  运行设置  -- 选择要运行的小程序的制定目录
// 微信小程序要设置  --- 安全   -- 开启服务端口
```



### 配置uview 

> uni app 最优秀的ui框架

```shell
// 官网有步骤
1. uView依赖SCSS，您必须要安装此插件

// 安装node-sass
npm i node-sass -D

// 安装sass-loader
npm i sass-loader -D

// 安装  如果没有配置文件先 npm init -y
npm install uview-ui
// 更新
npm update uview-ui

2. 引入文件
项目根目录中的main.js 
// main.js
import uView from "uview-ui";
Vue.use(uView);

引入uView的全局SCSS主题文件
根目录的uni.scss
/* uni.scss */
@import 'uview-ui/theme.scss';

引入uView基础样式 app.vue
<style lang="scss">
	/* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
	@import "uview-ui/index.scss";
</style>

配置easycom组件模式
项目根目录的pages.json
// pages.json
{
	"easycom": {
		"^u-(.*)": "uview-ui/components/u-$1/u-$1.vue"
	},
	
	// 此为本身已有的内容
	"pages": [
		// ......
	]
}
```

配置：[uview](https://www.uviewui.com/components/downloadSetting.html)

> 运行项目

```shell
// 如果报  ERROR  Error: Cannot find module 'webpack/lib/RuleSet'
webpack 没有安装
npm install webpack --save
```

> 如果一直报错的话，可能是sass版本太高，直接删除node_modules配置文件
>
> 修改版本
>
> `"node-sass": "^5.0.0",`
>
> `"sass-loader": "^10.1.1",`  

```shell
npm install //安装所有的依赖
```



### 安装请求插件

>  axios:

```
1. 创建一个services文件夹，创建request.js文件;

2. npm i luch-request -S    // 安装luch-request插件
```

> 3. requset.js

```js
import Request from 'luch-request'    //请求插件

const appConfig = "http://www.test.com"  //请求的地址 的 域名

const getTokenStorage = () => {
	let token = ''
	try {
		token = uni.getStorageSync('token')
	} catch (e) {
		console.log(e);
	}
	return token
}
const devHttp = new Request()

/* 设置全局配置 */
devHttp.setConfig((config) => {
	config.baseURL = appConfig
	config.custom.loading= true;
	config.header = {
		...config.header,
		'Content-Type': "application/json"
	}
	console.log(config);
	return config
})

/* 请求之前拦截器。可以使用async await 做异步操作  */
devHttp.interceptors.request.use((config) => {
	config.header = {
		...config.header,
	}
	if (getTokenStorage()) {
		config.header['token'] = getTokenStorage()
		// console.log(config.header['token'])
	}
	if (config.custom.loading) {
		uni.showLoading()
	}
	return config
}, (config) => {
	return Promise.reject(config)
})

//响应拦截器即异常处理
devHttp.interceptors.response.use((response) => {
	uni.hideLoading();
	let res = response.data
	let code = res.code
	let resultData = response.data.data;

	if (code !== 200) {
		// // 5001: 请求参数校验异常
		// if (code === 5001) {
			
		// 	uni.showToast({
		// 		title: errorMessage,
		// 		icon: 'none'
		// 	})
		// } else if (code === 51068) {
			
		// } else if (code === 51067) {
			
		// } else {
		// 	uni.showToast({
		// 		title: res.message || 'Error',
		// 		icon: 'none'
		// 	})
		// }
		// Promise.reject(new Error(res.message || 'Error'))
		// 当状态码不是200时，是否需要返回响应结果给调用方
		return res
	} else {
		return res
	}
}, (response) => {
	uni.hideLoading();
	console.log("response: " + JSON.stringify(response));
	const status = response.statusCode;
	if (status === 51067) {
		uni.showToast({
			title: "登录过期，即将跳转至登录页",
			icon: 'none',
			duration: 1000
		});
		
	} else {
		return Promise.reject(response)
	}
})
export {
	devHttp 
}
```

> 4. mian.js 引入配置文件

```js
import { devHttp } from '@/services/request.js'
Vue.prototype.$http = devHttp;
```

> 5. 新建  proxy  下 index.api.js

```js
import { devHttp } from '@/services/request.js'
let loginUrl = "/login/login";    //请求的地址
const $http=devHttp;

/**
 * 用户登录
 */
export const login = (data) => {
    return $http.post(loginUrl, data)
};

// 用户注册
let registUrl = 'login/regist';
export const regist = (data) => {
    return $http.post(registUrl, data)
};
```

> 6.页面引用

```js
// 在要用的页面引入  
import {regist} from '@/proxy/index.api.js'
import { login,dep } from '@/proxy/index.api.js'
 
onShow() {   //周期函数
    this.reg()  // 请求函数 
},
    
    // 请求函数
    async reg(){
        var data = {
            id: 1,
            username: 'admin'
        },
        var result = await regist(data)
        console.log(result)
    },
```

> 如果报跨域的情况

```js
1. 在目录 manifest.json 后配置跨域的信息
	"h5" : {
		"devServer" :{
			"port" :8080,
			"disableHostCheck" :true,  //检查主机
			"proxy": {
				"/api":{ 
					"target" : "http://"   ,// 后台
					"changeOrigin" :true,   //开启代理
					"secure" : false,		//检查安全
					"pathRewrite" :{        ///api部分
						"^/api":""
					}
				}
			}
		}
	}

2. 在tp项目后的入口文件设置跨域头部
header("Access-Control-Allow-Origin:*");
header("Access-Control-Allow-Methods:GET, POST, OPTIONS, DELETE");
header("Access-Control-Allow-Headers:DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type, Accept-Language, Origin, Accept-Encoding");
```







****

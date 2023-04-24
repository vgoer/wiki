---
title: 11.vue应用
description: vue应用
published: 1
date: 2023-04-24T10:46:28.634Z
tags: vue2
editor: markdown
dateCreated: 2023-04-24T10:46:28.634Z
---

<center>vue 应用</center>



## vue 面终端应用

> `cli` 命令行 (咱们可以会出现很多问题) 下面解决

> `uni-app` 和 `Hbuilderx`,面终端开发`vue`应用  框架 和 ide

```html
// 直接新建 -- 项目 -- uniapp
// 目录结构
```



```
// 打包 发行
发行，-- 网站 或H5  --  找到打包文件  -- 部署到域名下
```



> 插件市场 [插件](https://ext.dcloud.net.cn/)

```js
// 1. less插件

// 2. 新建 less文件
@base:#008c8c;  //变量
.box{
	color: @base;
	font-size: 30px;
}

// 引入less
<style lang="less">
	@import "../../test.less";
</style>
// 一定要加  lang="less" 
// 注意路径
// 结束 分号 ；
```



### uniapp

> 移动端ui框架 [uniapp](https://uniapp.dcloud.io/)

```js
// 多去 查看官方文档
```



> 组件之间传输数据

```vue
// 定义新组件 new.vue

<template>
	<div class="box">
		<h2>{{title}}</h2>
		<p>{{content}}</p>
	</div>
</template>

<script>
	export default{
		name:'new',
		props:{
			'title':{
				type:String,
				default:''
			},
			'content':{
				type:String,
				default:''
			}
		}
	}
</script>

<style>
</style>
重要
// props 传递属性  type:类型  default:默认值
```

```vue
// 父组件引用 注册
<template>
  <div id="app">
	<New title="hello" content="hello world"></New>
  </div>
</template>

<script>
import New from '@/components/new'

export default {
  name: 'App',
  components: {
	New
  }
}
</script>

<style>
</style>

```

> 组件之间互相传递数据 -- 一定要会

```vue
// props 我们就可以父组件传值给子组件
props:{
    'title':{
        type:String,
        default:''
    },
    'content':{
        type:String,
        default:''
    }
},
//title content --> 传递的值
<New title="hello" content="hello world" @textsex="textsex"></New>
```

```vue
// 子组件属性绑定
<h2 @click="sexf">{{title}}</h2>

methods:{
    sexf(){
    	console.log(this.title)
    	this.$emit('textsex',this.title)
    }
}
// $emit('绑定属性'，'值可以数组')
```

```vue
// 父组件 接受值
// 属性 绑定 值
<New title="hello" content="hello world" @textsex="textsex"></New>
//函数调用
methods: {
    textsex: function(e) {
    	console.log(e)
    },
},
```



> over 这里的vue完了，但是却才入门  ==加油==


---
title: 02.数据Date
description: 数据Date
published: 1
date: 2023-05-05T11:49:47.179Z
tags: vue2
editor: markdown
dateCreated: 2023-04-24T10:36:19.532Z
---

<center>数据与方法</center>

[toc]



### 数据Date

> 创建`vue`应用，数据和 DOM 已经被建立了关联，所有东西都是**响应式的**

```html
<div id="app">
{{name}}
</div>

<script src="../script/vue.js"></script>
<script>
var data = {'name':'vue'};
var app = new Vue({
el:'#app',
data:data,
});
// data.name = '------blue----vue----';
app.name = '------black---vue----';
//两种方法都可以修改name，重新渲染页面，响应的
</script>

//$data --> 获取自己的属性
app.$data == data  //true
```

> `==$watch== `方法查看变量变化  -- 常用

```js
app.$watch('name',function(newval,oldval){
	console.log(oldval,newval);
});
//放在变量变化中间
```
---
title: 02.js和jq练习
description: js和jq练习
published: 1
date: 2023-06-09T10:13:23.514Z
tags: javascript, jquery
editor: markdown
dateCreated: 2023-04-22T17:17:18.615Z
---

<center>练习</center>

[toc]

## js和jq练习



#### 1. 选项卡

> 使用jq完成xuanxiangka.html的选项卡效果

```html
<style type="text/css">
    *{padding:0;margin:0;}
    ul,li{list-style:none;}
    .list{height:30px;width:400px;border:1px solid #000;}
    .list li{float:left;width:100px;height:30px;line-height:30px;text-align:center;cursor:pointer;}
    .active{background-color:blue;color:#fff;}
    .content{height:350px;width:400px;border:1px solid #000;}
    .content .show{display:block;}
    .content ul{display:none;}
</style>

<ul class="list" id="list">
    <li class="active">新闻</li>
    <li>体育</li>
    <li>产品</li>
    <li>广告</li>
</ul>
<div class="content" id="content">
    <ul class="show">
        <li>新闻列表1</li>
        <li>新闻列表2</li>
        <li>新闻列表3</li>
        <li>新闻列表4</li>
    </ul>
    <ul>
        <li>体育列表1</li>
        <li>体育列表2</li>
        <li>体育列表3</li>
        <li>体育列表4</li>
    </ul>
    <ul>
        <li>产品列表1</li>
        <li>产品列表2</li>
        <li>产品列表3</li>
        <li>产品列表4</li>
    </ul>
    <ul>
        <li>广告列表1</li>
        <li>广告列表2</li>
        <li>广告列表3</li>
        <li>广告列表4</li>
    </ul>
</div>
```

```js
//<script src="jq库"></script>

$('#list li').click(function(){
  	var i=$(this).index();   //index()方法获取当前索引
	$(this).addClass('active').siblings().removeClass('active');
$('#content ul').eq(i).addClass('show').siblings().removeClass('show');
  })
```

#### 2. 隔行换色

> 用jq来做

```js
// ul下包含多个li
function showColor(){
    $('ul li:even').css('background-color','blue'); //偶数行
    $('ul li:odd').css('background-color','skyblue');//奇
}

$('ul li').mouseover(function(){
    $(this).css('background-color','white').sibings().css('background-color','green');
})
$('ul li').mouseout(function(){
  showColor();  
})
$('ul li').hover(funtion(){},function(){});
```



#### 3. 计算器

> 使用js和JQ分别完成计算器的效果:
> 		如果除数是0就使用红色字体输出'除数不能为0'
> 		其他情况就使用#666颜色字体输出

```html
<input type="number" value="" id="num1" />
<select name="" id="symbol">
    <option value="0" >加</option>
    <option value="1" >减</option>
    <option value="2" >乘</option>
    <option value="3" >除</option>
</select>
<input type="number" value="" id="num2" />
<input type="button" value="计算" id="count" />
<input type="button" value="jq计算" id="count1" />
结果为： <span id="res"></span>

```

```js
// js
count.click(function(){
    // 拿到num的value值，判断如果为空就赋值0
    var num11 = num1.value = ''?num1.value:0;
    var num22 = num2.value = ''? num2.value:0;
    var result = eval(num11 + symbol.value +num22);
    if(result == 'Infinity'){
        res.innerHTML = '除数不能为0';
        res.style.color = 'red';
    }else{
        res.innerHTML = result;
        res.style.color = '#666';
    }
})

// jq

$('#count1').click(function(){
    var num11 = $('#num1').val() = ''?$('#num1').val():0;
    var num22 = $('#num2').val() = ''? $('#num2').val():0;
    var result = eval(num11 +$('#ssymbol').val()+num22);
    if(result == 'Infinity'){
        $('#res').innerHTML = '除数不能为0';
     	$('#res').style.color = 'red';
    }else{
     	$('#res').innerHTML = result;
     	$('#res').style.color = '#666';
    }
})
```






















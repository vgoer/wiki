---
title: 01.标签
description: html
published: 1
date: 2023-04-24T10:31:22.159Z
tags: html
editor: markdown
dateCreated: 2023-04-19T17:51:10.000Z
---

<center>HTML</center>

[toc]

# html:

> 超文本标记语言（HyperText Markup Language，简称：HTML）是一种用于创建网页的标准*标记语言*。
>
> > 超文本 （文本 + 图片 + 视频 + 音频 + 链接）

```tex	
HTML5设计目的主要为了移动端支持多媒体

hmtl5支持firefox ie9或更高版本 chrome safari
```



### 1. 结构

![img](https://www.runoob.com/wp-content/uploads/2013/06/02A7DD95-22B4-4FB9-B994-DDB5393F7F03.jpg)

- **<!DOCTYPE html>** 声明为 HTML5 文档

* **<head>**包含了文档的元（meta）数据

  ```html
  		<meta charset="utf-8" />
  		<title>网页的标题</title>
  		<meta name="keywords" content="商城,购物,计算机" />
  		<meta name="description" content="内容介绍" />
  ```

  > SEO 搜索引擎优化 : 为了规范和专业 TDK（title,description,keywords） 必须要写
  >
  > SEM 搜索引擎营销 : 通常按照点击竞价付费，参考价几块到十几块

- **<body>** 元素包含了可见的页面内容

### 2.常用标签

> ​	用的频率比较高的，一般都有开始结尾的标签



##### 1. 注释

> 注释：==很重要==不影响页面，提高代码可读性

```html	
<!-- 请这里写注释呀！ -->
```



##### 2. 段落

> p标签：每一个段落都使用p标签,用来包含文本

```html	
<p>
    我是一个段落，请在这里输入文本内容等等。。。
</p>
```

| 属性                 | 值                                   |
| :------------------- | ------------------------------------ |
| align (文本水平位置) | left(左)  \| center(中) \| right(右) |



##### 3. 标题

> h1-h6标签：标题标签

```html
<h1>一级标题</h1>

<h6>六级标题</h6>

```

| 属性                 | 值                                   |
| :------------------- | ------------------------------------ |
| align (文本水平位置) | left(左)  \| center(中) \| right(右) |



##### 4. 文本格式化

* 换行符

  > `<br>` ：在需要**换行**的地方使用 

* <b>加粗</b>

  > `<b>和<strong>`：加粗文本

* <i>斜体</i>

  > `<i>` ：加斜体

* <del>删除线</del>

  > `<s>和<del>` : 加删除线

* 变<small>小</small>

  > `<small>` : 文本变小

* 变<big>大</big>

  > `<big>` : 文本变大

* <sup>上</sup>标

  > `<sup>` : 文本上标

* <sub>下</sub>标

  > `<sub>` : 文本下标



##### 5. 链接

> a : 超链接，可以点击跳转到任何 地方/网页

| 属性                    | 值                                                           |
| :---------------------- | ------------------------------------------------------------ |
| href (跳转地址)         | 网页\|   本地文件\|   mailtoi:123@da(邮箱)\|   tel:110(电话)\|   sms:120(发短信) |
| title(鼠标移动显示标题) | 任意                                                         |
| target(跳转方式)        | _self(默认，当前页面) \|   _black(新页面)                    |

```html	
<a href="www.baidu.com" target="_black">百度</a>

<a href="mailtoi:123@qq.com">发邮寄</a>
<a href="tel:007">拨号</a>
<a href="sms:007">发短信</a>
```



> 锚点：当前页面的任何位置

```html
<!-- 注意：name属性   href值加# -->
<h3 name="top"></h3>
......
<a href="#top">回到顶部</a>

```



##### 6.图片 

> img: 图片 

| 属性                               | 值   |
| ---------------------------------- | ---- |
| src(引入图片路径)                  | 任意 |
| alt(图片失效后显示文本)**SEO优化** | 任意 |
| height(图片高度)                   | 任意 |
| width(图片宽度)                    | 任意 |

```html
<img src="https://baidu.com" alt="百度图片" height="200">
```



> eg: 图片不要同时设置宽高，不然会变形

**==小拓展==：**

> a. 分辨率和像素

```tex	
分辨率: 单位长度内像素点的数量 单位（ppi=像素/英寸)
	72ppi表示：1英寸包含72个像素点
eg:分辨率月高，像素就越高，图像越清晰

像素：在由一个数字序列表示的图像中的一个最小单位，称为像素。分辨率的尺寸单位。
eg:像素点更小，像素的密度更高，所以可以呈现更多细节和更多细微的颜色过度效果。

*** 1英寸=2.54厘米 ***

对角线： c = a平方(宽)+b平方(长)>开根
c / 2.54  ==> 屏幕的尺寸

ppi的全称是Pixels Per Inch，指的是每一英寸上的像素数目，ppi越高，图像显示的密度就越高，就越清晰。
```



##### 7.音频

> audio: 带控制器的音频播放器  (**html5的**)

==source==: 音频和视频标签的媒介标签，**为了更好的兼容**

| property  | value                              |
| --------- | ---------------------------------- |
| src(路径) | 任意                               |
| controls  | 控件(值==属性)                     |
| autoplay  | 自动播放                           |
| loop      | 循环播放                           |
| preload   | 播放前加载(设置该值，autoplay失效) |

```html
<audio contrlos autoplay loop>
	<source src="music.mp3" type="audio/mp3">
	<source src="music.ogg" type="audio/ogg">
</audio>
```



##### 8.视频

> video:视频播放器  (**html5的**)

==source==: 音频和视频标签的媒介标签，**为了更好的兼容**

| property               | value    |
| :--------------------- | -------- |
| src(路径)              | 任意     |
| width(宽)              | 任意     |
| height(高)             | 任意     |
| controls(视频控件)     | 自己     |
| autoplay(自动播放)     | 自己     |
| loop(循环播放)         | 自己     |
| preloade(播放之前加载) | 自己     |
| muted(静音播放)        | 自己     |
| poster(视频封面图片)   | 图片地址 |

```html	
<video width="300" controls autoplay loop muted poster="one.jpg">
	<source type="video/mp4" src="video.mp4">
    <source type="video/ogg" src="video.ogg">
</video>
```



##### 9.实体字符

> 特殊字符和被标签占用的字符

| 字符           | 表示    |
| -------------- | ------- |
| 大于(>)        | \&gt;   |
| 小于(<)        | \&lt;   |
| 引号("")       | \&quot; |
| 注册商标 &reg; | \&reg;  |
| 版权 &copy;    | \&copy; |
| &号            | \&amp;  |

​	


---
title: 13.Reg正则表达式
description: Reg正则表达式
published: 1
date: 2023-04-22T17:35:48.370Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:35:48.370Z
---

<center>re</center>

[toc]

### 正则表达式

> 一套专门用于**处理文本的强大工具**,可以对进行文本查找，匹配，替换;  Perl风格



#### 1.preg_match()

> 正则表达式匹配  只能匹配到一个

```php
//  参数1：匹配规则
//  参数2: 匹配字符串
//  参数3: 返回的值放到变量内  数组
$str = 'hellh wo';
preg_math("/h/",$str,$res);
var_dump($res); //数组 h

// 如果没有第三个参数 就返回  0  1  false  true
```



#### 2.preg_match_all()

> 和上一个参数一样， 但是是  全局匹配 **匹配所有**

```php
preg_match_all("/h/",$str,$res);
// 二维数组 ，匹配到两个h
```



#### 3. preg_replace()

> 执行正则表达式的  **替换**

```php
//  参数1：匹配规则
//  参数2: 替换的字符
//  参数3: 匹配字符串
$str = "helll bbbl";
echo preg_replace("/l/","a",$str);   //heaaa bbba
```



#### 4.preg_grep()

> 用正则表达式分割字符串

```php
// 参数1：匹配规则
// 参数2：字符串

$str = 'he he wo ji';
$res = preg_grep('/ /',$str);  //空格分割
// 数组
array(
	[0]=>he,
    [1]=>he...
)
```



#### 5.preg_grep()

> 返回匹配数组的元素   返回值是数组

```php
$arr = array(111,222,333,444,555,111,333);
$res2 = preg_grep("/111/",$arr);

p($res2);
// 数组 
Array
(
    [0] => 111
    [5] => 111
)
```



#### 6. 元字符

> `{}`  匹配个数

| 字符  | 描述                              |
| ----- | --------------------------------- |
| {m}   | m 是一个非负整数。匹配确定的 m 次 |
| {m,}  | m 是一个非负整数。至少匹配m 次    |
| {m,n} | 最少匹配 m次且最多匹配 n次        |
| ()    | 表示一个整体                      |
| .     | 匹配除换行之外的任何一个字符      |

* `*`   零次或多次     等同于 `{0, }`

```php

```












---
title: 06.Date日期
description: Date日期
published: 1
date: 2023-05-05T11:52:37.814Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:27:04.609Z
---

<center>日期函数</center>

[toc]

## 日期函数

> Date/Time 函数允许您从 PHP 脚本运行的服务器上获取日期和时间。



#### 1. 时间戳

> * 返回自从 Unix 纪元（`格林威治时间 1970 年 1 月 1 日 00:00:00`）到当前时间的秒数。

```php
echo time();  //1970 1 1到现在的秒

//当前时间戳时间
echo (date("Y-m-d H:i:s",time()));
```



#### 2. 默认时区

> * `date_default_timezone_get() `函数==返回==脚本中所有日期时间函数所使用的默认时区。

```php
echo(date_default_timezone-get());
```

> * `date_default_timezone_set() `函数==设置==用在脚本中所有日期/时间函数的默认时区。

```php
echo(date_default_timezone_set("Europe/Paris"));
```



#### 3.获取时间

> * `date() 函数格式化一个本地时间／日期。`

```php
date('Y-m-d H:i:s',time());
```



> * `getdate() 函数取得日期／时间信息。` 

```php
echo getdate();  //。如果没有给出时间戳，则认为是当前本地时间。
echo getdate(time());
```



> `idate() 函数将本地时间/日期格式化为整数。` 只能格式一个

```php
echo idate("Y",time()); //只接受一个字符 为参数
echo idate("s",time());
```



> * `localtime() 函数返回本地时间（一个数组）。`

```php
var_dump(localtime());
```



> `microtime() 函数返回当前 Unix 时间戳和微秒数。`

```php
//返回时间戳和微秒数
echo microtime();
```



> `mktime() 函数返回一个日期的 Unix 时间戳。`

```php
//  参数  hour minute  second month  day year
echo (5,10,59,5,10,2021);  //时间戳
```



> ```
> strftime() 函数根据区域设置格式化本地时间／日期。
> ```

> ```
> strptime() 函数解析由 strftime() 生成的日期／时间。
> ```

```php
echo(strftime("%b %d %Y %X", mktime(20,0,0,12,31,98)));

$format="%d/%m/%Y %H:%M:%S";
$strf=strftime($format);
```



> *重要*``strtotime() 函数`将任何英文文本的日期时间描述解析为 Unix 时间戳。

```php
// str = 2020/10/2 10:30:1   表单获取到的数据
echo(strtotime($str)); //时间戳
//文字描述
echo strtotime(now);
echo strtotime(3 October 2020);
echo(strtotime("+1 week 3 days 7 hours 5 seconds"));
echo strtotime("next year");
```

时间戳转换[date](https://tool.lu/timestamp/)



```php
// 设置时区
//设置时区,中华人民共和国时区PRC
//date_default_timezone_set('PRC');
date_default_timezone_set('PRC');

```


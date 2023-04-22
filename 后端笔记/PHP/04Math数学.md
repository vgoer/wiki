---
title: 04.Math数学
description: Math数学
published: 1
date: 2023-04-22T17:26:13.551Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:26:13.551Z
---

<center>数学函数</center>

[toc]

## 数学函数

> 计算处理整型和浮点型的值

1. abs()

   > 绝对值:不用多说吧 

```php
echo abs(-1);  //1
```

2. ceil()

   > 向上取整

```php
echo ceil(0.999999);  //1
echo ceil(0.00000001); //1
```

3. floor()

   > 向下取整

```php
echo floor(0.9999); //0
echo floor(9.9999); //9
```

4. max(a,b)

   > 两个数 最大值

```php 
echo(max(10,9)); .//10
```

5. min(a,b)

   > 两个数 最小值

```php
echo(min(1,4)); //1
```

6. mt_rand(a,b);

   > min,max 中的 返回随机整数。

````php
echo mt_rand(10,20);  //10到20内的随机数
````

7. pi()

   > pi() 函数返回圆周率的值。

```php
echo pi();
```

8. pow()

   > pow(x,y)函数返回 x 的 y 次方。

```php
echo pow(2,10); //1024
```

9. rand()

   > rand() 函数返回随机整数

```php
echo rand(10,20); //10到20内的整数 
```

10. round()

    > 函数对浮点数进行四舍五入。

```php
echo round(10.6); //11
```
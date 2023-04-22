---
title: 02.Function函数
description: Function函数
published: 1
date: 2023-04-22T17:28:41.499Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:21:11.222Z
---

<center>函数</center>

[toc]

## 函数

> 定义一个功能和方法

```php
function sum($a,$b=40){  //形参，和默认值
	return $a+$b;		//return 返回值
}
echo sum(10,30);

// PHP的函数名禁止重名,包括内置函数和自定义函数
```



函数作用域

> 函数内部和外部是两个世界,外面的变量和里面的变量完全没有关系
>
> > 和js区别很大

```php
// 函数里面访问不了外面，外面也不能访问里面

//1.如果函数外部变量放到函数内部使用

//a.传参
$a = 10;
function aa($can){
    echo $can+1;
}
aa($a);  //$a传入函数

//b global :声明变量可以在函数内部使用
$a = 10;
function aa($can){
    global $a;   //声明变量
    echo $can+1;
}
aa();  //$a传入函数


// 2.里面的变量放到外面只能靠 return
function f1(){
    $a = 100;
    return $a;
}
$b = f1(); 

// 3.静态变量 ；调用一次函数变量会执行一次
function f2(){
    static $i=0;
    $i++;
    echo $i;
}
f2();  //1
f2();  //2
f2();  //3
```



### 函数库

> 将所有的函数单独方瑞一个文件内，这个文件就是函数库

>  引入函数库的方法(引入php文件的方法)

```php
include() //路径   即使引用错误，也不会影响下面代码的继续输出
   
require()  //路径  引入错误,会终止页面所有代码继续执行

//为了避免重复引入产生的报错,可以使用include_once和require_once来代替
// 可以引入多个 include_once() require_once()
```



#### 递归函数

> 重复调用自己

```php
// 累加
fucntion addSup($num){
   if($sum == 1){
       return 1;
   }else{
       return $num+addSup($num-1);
   }
}

addSum(10); //10的累加
```



#### 回调函数

> 就算是把函数当参数传入

```php

echo addSup(10); //55
// 10 9 8 7 6 5 4 3 2 1 

function addSub($a,$b,$fun){
    return $a+$b+$fun($b); //把上面的函数当参数传入
}

echo addSub(1,10,'addSup'); //66

```



#### 变量函数

> 可变函数仅仅是可变变量的一个变种、变形表达。

```php
// 变量的变量
$($b);

function dome(){
    echo 'googog';
}

$fun = 'demo';
$fun();   //这样就可以调用了
//  可以用于以后的MVC
```



#### 匿名函数

> 也就是没有函数名的函数   直接调用

```php
$hello = function($name){
    echo $name."欢迎你";
}; //分号一定要有

$hello("kangkang");

```



#### 内部函数

> 内部函数，是指在函数内部又声明了一个函数。

```php
function supbox(){
    echo "i'am supbox";
    
    function subbox(){
        echo "i'am subbox";
    }
}

// 必须先调用最外层的函数，才能调用内部函数
supbox();

subbox();
```



#### 闭包函数

> php的闭包（Closure）也就是匿名函数。是PHP5.3引入的。

```txt
1 减少foreach的循环的代码
2 减少函数的参数
3 解除递归函数
4 关于延迟绑定
```

```php
//关键字就只有use，use意思是连接闭包和外界变量

$mes = 'hello';
$res = function() use ($mes){
    var_dump($mes);
}
// 这样我们不能改变传入的参数
$res();
$mes = 'word';
$res(); // 值不会变



$res = function() use (&$mes){ //加一个&符
    var_dump($mes);
}
//这有重复改变值
```


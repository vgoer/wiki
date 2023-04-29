---
title: 01.PHP简介
description: PHP简介
published: 1
date: 2023-04-29T17:47:42.093Z
tags: php
editor: markdown
dateCreated: 2023-04-19T17:52:40.411Z
---

<center>php基础</center>

[toc]

##  PHP基础



### 1. 环境搭建

> 搭建的环境：我们用小皮就行

官网：[小皮](https://www.xp.cn/)

这样我们就可以直接用`php、mysql`等等

> 操作最多的就是添加网站   ==设置域名和端口== 

![image-20210720100144521](W:\notes\front-end\5.PHP\image-20210720100144521.png)

> 设置好域名，端口，选择版本，我们就可以开始了



### 2. 注意

> php注意事项：
>
> 			1. 每一行必须要写分号 `；`
> 			2. 要  设置编码

```php
// 分号和设置编码

header("Content-Type:text/html;charset=utf-8");
```

> 注释：方便阅读和修改

```php
//单行注释
// 单行
#  单行

// 多行注释
/*  多行  */

//文档注释
/**
 *  什么函数
 *  参数1
 *  参数2
 */
```



### 3. 变量

> 变量：代码精简、方便维护阅读
>
> **$ 声明变量**

```php
$a = 'abc';
// `echo`打印输出  . 拼接

echo $a."<br>";
```

变量的引用赋值

```php
// 变量的引用赋值

$a1 = 200;
$a2 = &$a1; //  两个变量有值的话永远相等

unset($a1)  //删除变量
```

变量的变量

```php
// 变量
$go = 'hi';
$hi = 'go';
echo $$go;  // 打印go 
echo $($hi); // hi
```



> 命令规范：

```html
PHP变量名的规范
1、变量以美元符$开头,然后是变量名
2、变量名由字母或下划线开头,整个变量名由字母、下划线或数字组成
3、变量名区分大小写
4、变量名可以不用声明就直接使用,但是最好提前声明或定义

```

```php
$echo='echo';
echo $echo;  //PHP变量没有关键字保留字
```



##### 超全局变量

> 很多有用的预定义变量，用于提供大量与环境有关的信息。

```html

(1) $_SERVER 服务器变量  该全局变量包含着服务器和客户端配置及当前请求环境的有关信息
    $_SERVER['SERVER_NAME']; //当前运行脚本所在的服务器的主机名
    $_SERVER['REMOTE_ADDR']; //客户端IP地址
    $_SERVER['REQUEST_URI']; //URL的路径部份
    $_SERVER['HTTP_USER_AGENT']; //操作系统和浏览器的有关信息
	//很多 直接打印出来 print_r($_SEWRVER)

2) $_GET   该变量包含使用 GET 方法传递的参数的有关信息;

(3) $_POST  该变量包含使用 POST 方法传递的参数的有关信息;

(4) $_REQUEST 该变量记录着通过各种输入方法传递给脚本的变量，如GET POST,但不要用这个超级全局变量，因为它不安全而且速度比较慢;
(5) $_COOKIE  cookie变量数组
(6) $_SESSION 会话变量数组
(7) $_FILES 与上传文件有关的变量数组
(8) $_ENV 环境变量数组
(9) $GLOBALS 有全局变量数组
```



##### 表单数据提交

> 表单属性：
>
>    1. method 提交方式：
>
>           get 一般用于搜索和删除等  
>
>          post 一般用于：注册，登录，增加，编辑，上传文件
>
>          		2. action 提交地址：
>
>          默认当前目录，填地址
>
>          		3. **enctype="multipart/form-data" ** 文件上传==必须==要表单加这个属性    而且是 **post**

```HTML
<form action="a.php" method="post" enctype="multipart/form-date">
    用户名：<input type='text' name="username">
    密码：<input type='password' name="password">
    <input type="file" name="fileimg">
   	<input type="submit" vlaue="login">
</form>
// get 在url中提交
// post 在请求主体中提交
// enctype="multipart/form-date" 提交文件必须要写
```

```php
// a.php
<?php
    header('Content-Type:text/html;charset:utf-8');
	
 	$username = $_POST['username'];   
    $password = $_POST['password'];
	
	//格式化数据函数
    fucntion pra($arr){
        echo '<pre>';
        print_r($arr)
        echo '</pre>';
    }
    
	pra($_POST);   //拿到用户名和密码
    pra($_FILES); // 拿到上传文件的信息
 
?>
```



### 4. 数据类型

1. 字符串

> 用单引号和双引号包含起来的任何字符

```php
// 注：单引号包含的值全是字符串，变量不能解析
// 双引号可以解析变量
$str = '123dsafa';
$str2 = "123abc{$str}";  //可以解析变量 一般使用大括号
```

> 转义符：特殊功能的字符转换为普通字符

```php
// 转换位普通字符
echo "i\'am lili'";  //加反斜杠  \$

//换行
echo "aaa<br>cc";  //html换行
echo "aaa\r\ncc";  //文本换行 \r\n

echo "aaa
bbb
ccc"      //输出多行s's's
```

2. 整型

> 整型和浮点型

```php
$nmu1 = '100';
$num2 = 100;
echo $num1+$num2;   200  //自动转换数据类型  

$num3 = '1abc123'; 
//字符串转换为整型,从左往右一个一个字符的转换,碰到非数字就立刻停止转换
echo $num3+$num1; //101  自动转换 
//如果两个字符串，只能点（.）拼接
```

`var_dump()`打印数据类型和值

3. 布尔值

> true和false

```php
var_dump(100>300); // boolean false
/* 
	以下值被认为是false
	整型0
	浮点型0.0
	空白字符串和字符串'0'
	空数组
	空对象
	特殊数据类型NULL
*/
```

4. NULL

> 空值，没有定义或者声明的变量返回null

```
echo $bb;  //没有定义
$cc;
echo $cc; //没有赋值 null
```

#### 判断类型

> 判断类型

```php
	/*  返回true false
		is_string 是否是字符串
		is_int 是否是整型
		is_float 是否是浮点型
		is_bool 是否是布尔型
		is_array 是否是数组
		is_object 是否是对象
	*/
$str = false;
echo is_bool($str)  
```



### 4. 运算符

> 和js差不多,不多说了

```
算术运算符 + - * / %
增量减量运算符 ++ --
比较运算符 > < >= <= == !=(<>) === !== 不等于两个

//逻辑运算符
	//与,&&,并且,一假即假,全真即真
	//或,||,或者,一真即真,全假即假
	//非,!,取反,真即是假,假即是真

//三元运算符
	//条件?条件为true执行的代码:条件为false执行的代码
$b= $b>10 ? 20 : 20;
```



### 5.流程控制

> 判断

```php
//if...else 范围判断和定点判断   一般范围
if(){
    true;
}else(){
    false;
}

//switch 定点判断
switch(){
    case a:
        echo $a;
        break;
     case a:
        echo $a;
        break;
    default:  //否则
        echo $a;
}
```

> 循环   都用`起始值+条件`

```php
// while
while(判断){
    条件成立；
}
	//while最少输出0次
	//do...while最少输出1次

//do ..while
$a=1;
do{
   //echo $a;  //1 2 3
    $a++;
}while($a>4);

//for
for($i=1; $i<5;$i++){ 
    echo $i; //1 2 3 4  //两个判断条件，前面一个无效。
}
```

打印函数的区别

```php
//echo 不能输出数组,可以输出多个数据
//print() 输出数据,只能输出一个数据
//var_dump()可以输出数组
//print_r()输出数组

//打印格式化的数组
echo "<pre>";
print_r($arr);
echo "</pre>";
```

```php

//foreach循环,循环数组和对象
//箭头左边叫键值,如果键值没有自定义,使用默认的从0开始的计算的整数,那么这个键值也叫索引,该数组也叫索引数组箭头右边叫值

$arr = ['a','b','c']; //索引数组
foreach($arr as $k=>$v){
    	//$arr是数组的变量名称
		//as是固定写法
		//$k和$v都是自定义的变量 需要什么就拿什么
		//$k是键值,$v是值
    echo $k.'--'.$v.'<br>';
}

//  关联数组：键值是自定义字符串的数组
$arr2 = [
    'one' => 'aaaa',
    'two' => 'bbbb',
    'three' => 'cccc'
];
foreach($arr2 as $value){
    echo $value."<br>"; //拿到值
}
```

### 6.数组

> 上面学到索引数组和关联数组

1. `count()` 获取数组个数

```php 
echo count($arr2); // 3
```

2. `mt_rand()`输出随机数

```php
echo mt_rand(10,20); //输出10-20之间的随机数，包含自己
// 随机输出数组的一个值
echo $arr[mt_rand(0,count($arr)-1)];
```

数组的增删改查

```php
$arr2 = [
    'one' => 'aaaa',
    'two' => 'bbbb',
    'three' => 'cccc'
];
// 查
print_r($arr2); var_dump();

// 添加
$arr2['fore'] = 'dddd';  //键值

//编辑 
$arr2['fore'] = 'ffff';  //直接修改

//删除
unset($arr2['fore']);  //unset删除
```

3. `range()`生成索引数组

```php
// range(a,b,c) //起始值，终点值 步长

$arr = range(0,10,2);   // 0 2 4 6 8 10

$arr2 = range('a','z',2); //a c d e g i k...

$arr3 = range('K','Q'); // K L M N O P Q
```

```php
//打印
function praStart($con){
    // 打印等腰三角形   $con行 
    for($i=1; $i<=$con; $i++){ // 1 2 3 4 5
        for($a=1; $a<=$con-$i;$a++){ // 4 3 2 1 0
            echo "<sapn>&nbsp;</span>";
        }
        for($b=1;$b<=(2*$i-1);$b++){ //1 3 5 7 9
            echo "*";
        }
        echo "<br>";

    }

}
    //praStart(100);
```

4. `in_array($str,$arr)`判断是否在数组内

```php
$arr2 = [
    'one' => 'aaaa',
    'two' => 'bbbb',
    'three' => 'cccc'
];
var_dump(in_array('one',$arr2));  // bool(true)
```

> 定义数组，然后循环到html中

```php
<!-- 
    高级分离术  逻辑和表现分离
-->


```



### 定界符

```php
$str =<<<DOF
<a target="_blank" href="http://b.com/b.php?session=$currentSessionID">跳转</a>
DOF;   // 自动转换引号 和变量

exit; 
die;
// 终止代码执行
```


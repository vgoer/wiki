---
title: 01.OOP对象
description: OOP对象
published: 1
date: 2023-05-29T04:16:27.884Z
tags: php, oop
editor: markdown
dateCreated: 2023-04-22T17:57:01.500Z
---

<center>oop</center>

[toc]

## oop

> 面向对象编程



### 面向过程

>  分析出解决问题的**步骤**，然后逐步实现。

1. 公式：程序 = 算法 + 数据结构

2. 优点：所有环节、细节自己掌控。

3.  缺点：考虑所有细节，工作量大。



### 面向对象

> 找出解决问题的人，然后分配职责。

1. 公式：程序 = 对象 + 交互

2. 优点

(1) 思想层面：

-- 更接近于人的思维方式。

-- 有利于梳理归纳、分析解决问题。

(2) 技术层面：

-- 高复用：对重复的代码进行封装，提高开发效率。

-- 高扩展：增加新的功能，不修改以前的代码。

-- 高维护：代码可读性好，逻辑清晰，结构规整。



#### 类和对象

> 类：一个抽象的概念，类是创建对象的模板
>
> 对象：类的具体实例

> **类之间是行为不同，类是抽象的概念，  对象之间是数据不同， 有具体的值。**

```
例如：(1)学生student是一个类，具有姓名，年龄等数据；
	具有学习study，工作work等行为。
对象：悟空同学，28岁。
      八戒同学，29岁。
(2)车 car是一个类，具有类型type，速度speed等数据
    启动start，停止stop，行驶run等行为。
对象：宝马，180.
	   比亚迪，100.
 
(3)狗dog是一个类，具有类型，姓名，重量weight等数据，
拉臭臭shit，玩play等行为。
              	 对象：拉布拉多，米咻。
		             金毛，赵金多
(4)字符串str是一个类，”abc”是一个对象。
```



> 定义类和实例

```php
// a.php
namespace aa
// 命名空间：防止类名冲突
class Test{
    private $name = 'aaa'; //私有属性，只能在内部调用
    public $age = 20; //公共属性，都可以访问
		
    public function say(){
        echo 'im aaa'
    }
}
```

```php
// b.php
namespace bb
class Test{
	public $name = 'bb';
    
    private function say(){
        //私有属性，只能在类内部调用
        echo 'im bb';
    }
}
```

```php
//test.php
//同时引用
include_once('a.php');
include_once('b.php');

// 命名空间 -- 实例化
$t1 = new aa\Test;  //实例化
$t2 = new bb\Test;

echo $t1->age; //访问公共属性
$t1->say();  //访问公共方法
```



##### this

> ```php
> 我想在对象的内部,让对象里的方法访问本对象的属性
> ```
>
> PHP里面给我们提供了一个本对象的引用`$this`;

```php
class test{
    public $name; //声明和公共属性
    function say(){
        echo $this->name; //对象内访问对象属性
    }
}
```



##### counst

> 类中定义常量:  对象的整个生命周期中都保持不变的值

```php
class math{
    count PI = 3.1415; //定义一个常量
	echo self::PI; //类内部访问    
}

echo math::PI;  //不用声明
```



##### static 

> ` static  `声明类属性或方法为静态,就可以不实例化类而直接访问    

```php
class Test{
    static $a = 'aaa';
    static function my(){
        return self::$a; //内部访问静态属性
    }
}
// 静态属性不可以由对象通过 -> 操作符来访问。    
//不需要实例化
echo Test::$a;  //外部访问属性
echo Test::my(); //外部访问方法

静态的成员属于类所有,所以我们在静态方法里,不能使用$this来引用静态成员.
建议使用self关键字来调用。
```


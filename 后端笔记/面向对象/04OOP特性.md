---
title: 04.OOP特性
description: OOP特性
published: 1
date: 2023-06-09T10:16:32.916Z
tags: oop, php
editor: markdown
dateCreated: 2023-04-22T18:08:38.570Z
---

<center>基本概念</center>



[toc]

## oop

> oop的三个基本概念：`封装、继承、多态`



### 封装

> 隐藏类的实现细节,让使用者只能通过事先定义好的方法来访问数据.
>
> ```php
> 对象以外的部分不能随意存取对象的内部数据(属性)
> ```

```
1. 简化编程，使用者不必了解具体的实现细节，只需要调用对外提供的功能。
2. 松散耦合，降低了程序各部分之间的依赖性。
3. 数据和操作相关联，方法操作的是自己的数据。
```

```php

class Stu{
    private $name;  //私有变量
    private $age;   //私有变量

    public function set_age($a){
        $this->age = $a;
    }

    public function get_age(){

        return $this->age;
    }
}

$black = new Stu;

$black ->set_age(30);
echo $black ->get_age();
```





### 继承

> ```php
> 继承是子类自动共享父类的数据结构和方法的机制,
> ```

> `extends `子类继承父类所有的方法和属性

```php
//子类继承父类的属性和方法
class People{
    public $name;
    private $age;

    function __construct($name)
    {   
        $this->name = $name;
    }
    public function set_age($a){
        return $this->age = $a;
    }

    public function get_age(){
        return $this->age;
    }

}
class Stu extends People{

    private $teil;

    public function set_teil($a){
        $this->teil = $a;
    }

    public function get_teil(){

        return $this->teil;
    }
}
$red = new Stu('小红');

echo $red->name;
$red->set_age(10);
echo $red->age; //报错，私有属性不能
echo $red->get_age(); 
```



### 多态

> 多态主要是基于继承和重写，最终可以实现相同的**类型调用相同的方法，结果不相同**

```
例如：有一个动物类，里面有一个方法输出，动物吃食物

小猫继承动物类后，重写这个方法输出，小猫吃鱼干

小狗继承动物类后，输出小狗吃骨头
```

```php
class Animal
{
    public $name;
    private $age;
    protected $color;

    public function __construct($name)
    {
        $this ->name = $name;
    }

    public function eat(){
        echo '我是'.$this->name.'在吃东西';
    }
}


class Cat extends Animal
{
    public function eat(){
        echo '我是'.$this->name.'在吃饼干';
    }
}


class Dog extends Animal
{
    public function eat(){
        echo '我是'.$this->name.'在吃骨头';
    }
}

function test($obj){
    $obj->eat();
}

test(new Dog('小汪'));
test(new Cat('小猫'));
// 我是小汪在吃骨头
// 我是小猫在吃饼干
```



### 对象链

> 实例化一个对象后，**连续的调用多个方法成员**。使用`return this表示本对象`

```php
class Bottle
{
    public $name;
    public $color;

    public function __construct($name,$color)
    {
        $this->name = $name;
        $this->color = $color;
    }

    public function name(){
        echo '我是'.$this->name;
        return $this;
    }

    public function sleep(){
        echo '我要睡觉';
        return $this;
    }

    public function color(){
        echo '我是'.$this->color;
        return $this;
    }

}
$blue = new Bottle("小蓝",'蓝色');
$blue ->name()->color()->sleep();
// 连续调用多个方法
```

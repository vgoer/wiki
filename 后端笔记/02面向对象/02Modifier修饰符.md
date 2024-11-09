---
title: 02.Modifier修饰符
description: Modifier修饰符
published: 1
date: 2023-05-29T04:16:29.314Z
tags: oop, php
editor: markdown
dateCreated: 2023-04-22T18:01:02.735Z
---

<center>修饰符</center>

[toc]

## 访问修饰符

```php
访问修饰符允许开发人员对类成员的访问进行限制,这是PHP5的新特性。

public 公共修饰符
     类的成员将没有访问限制,所有的外部成员都可以访问(读和写)这个类成员。
     在属性或方法前面加上关键字public,或不加任何关键字,都可以声明一个公共属性或方法。

private 私有修饰符
     被定义为private的成员,对于同一个类的所有成员是可见的,即没有访问限制;
     但对于该类的外部代码是不允许改变甚至读操作,对于该类的子类也不能访问。

protected 保护修饰符
     被修饰为protected的成员不能被该类的外部代码访问。
     但是对于该类的直接子类有访问权限,可以进行属性、方法的读及写操作。
     被子类继承的protected成员,在子类外部同样不能被访问
```



### public

> 公共的：`类的成员将没有访问限制` 不写public 也是公共属性

```php
class Dome{
    public $name = 'aaa';  //公共属性
    function dome(){       //公共方法
        
        return $this->name;  //内部访问自己属性
    }
}
//实例化
$dome1 = new Dome();

echo $dome->name; //外部调用属性
echo $dome->dome(); //外部调用方法
```



### private

> 私有修饰符 : 类外部无法访问  `不能继承`

```php
// 私有属性不会继承
class dome1{ 
    private $name = 'ccc'; //私有属性

    private function myname(){  //私有方法
        return $this->name;
    }
    public function bb(){
        return $this->name;  //私有属性只有在内部访问
        // $this->myname();  
    }
}
$dome1 = new dome1;

// $dome1->name; //报错 私有属性和方法外部无法访问 

echo $dome1->bb(); //这样才可以 访问
```



###  protected

> 保护修饰符:  成员不能被该类的外部代码访问。
>
> ```php
> 该类的直接子类内部有访问权限,可以进行属性、方法的读及写操作。
> ```

```php
// 保护属性可以继承到子类
class one{
    public $name = 'one';
    protected $age = 30;

    function test(){
        return $this->age;  //可以访问保护属性
    } 
}

//创建类，继承 one类
class two extends one{
    public function test2(){
        return $this->age; //继承的子类内部可以修改保护属性
    }
}

$one = new one;
// echo $one->age;   //报错保护属性
echo $one->test();   //可以访问包含属性

$two = new two;//子类
echo $two->test2();  //可以访问包含属性
```

> 保护和私有 都只能在**内部访问**，外部访问报错
>
> 私有属性，不能继承，否则为空
>
> 保护属性，可以继承


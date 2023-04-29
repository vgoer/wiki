---
title: 03.Magin魔术方法
description: Magin魔术方法
published: 1
date: 2023-04-29T17:48:22.839Z
tags: oop, php
editor: markdown
dateCreated: 2023-04-22T18:07:23.640Z
---

<center>魔术方法</center>

[toc]

## 魔术方法

>  所有以` __（两个下划线）开头的类方法`保留为魔术方法。
>
> ```
> 所有的魔术方法 必须 声明为 public。
> ```

> 好好学，很*序列化和反序列化*有关



### 构造函数__construct()

> 对象实例化 个属性初始化值
>
> 对象实例化调用

```php
// 通常我们希望在对象实例化时可以初始化某些属性,或执行某些方法
//当然,可以在对象实例化之后再这么做  为了方便

// 构造函数,在创建对象的时候调用
namespace Stu;
class Stu{
    function __construct($name,$age) 
    {
        $this->name = $name;
        $this->age = $age;
    }
    public function say(){
        return $this->name.'++++'.$this->age;
    }    
}

$stu1 = new Stu('张三',20); //初始化给定的属性
echo $stu1->say();
```



### 析构函数 __destruct()

> 结束函数，自动调用
>
> 析构函数允许在销毁一个类之前执行的一些操作或完成一些功能,

```php
// 类结束自动调用  脚本关闭时被调用
class test{
    public function __destruct()
    {
        echo '类结束自动调用';
    }
}
$test =  new test();
// 类结束自动调用  --会输出
```



> ```php
> //魔术方法时面向对象特有的
> 特定的情况下被触发，都是以双下划线开头，你可以把它们理解为钩子。
> ```



### __set()

> 当给**不可访问或不存在属性赋值**时被调用

```php
// 赋值
class Cat
{   
    private $name;

    public function __set($name, $value)
    {
        echo "自动执行了test类中的__set方法,当前属性为:{$name},值为:{$value}";
    }
}

$blue = new Cat;
$blue -> name = '小'; //被调用
// $blue->name = '小蓝'; //被调用
```



### __get()

> **读取不可访问或不存在属性**时被调用

```php
// 读取
class Cat
{
    private $name;

    public function __get($name){
        echo "不可以访问";
    }
}
$blue = new Cat;
$blue ->$name; //私有的属性  被调用
$bleu ->$age; //不存在的属性    被调用   
```



### __call

> **调用不可访问或不存在的方法**时被调用

```php
// 调用 方法
class Cat
{
    public function __call($name, $arguments)
    {
        echo '你想调用我不存在的方法', $name, '方法<br/>';
        echo '还传了一个参数<br/>';
        var_dump($arguments);  
    }
}
$blue = new Cat;
$blue ->say(1,2,3);  //调用了一个不存在的方法

//你想调用我不存在的方法say方法
//还传了一个参数
//array(3) { [0]=> int(1) [1]=> int(2) [2]=> int(2) }
```



### __callStatic

>**调用不可访问或不存在的静态方法**时被调用

```php
// 静态方法
class Cat{
    /**
         调用不可访问或不存在的静态方法时被调用
         魔术方法__callStatic（）必须具有公共可见性并且是静态的
    **/
    public static function __callStatic($name, $arguments)
    {
        echo '你想调用我不存在的方法', $name, '方法<br/>';
        echo '还传了一个参数<br/>';
        var_dump($arguments); 
    }
}

// $blue = new Cat();
$blue::say(1,2,3);   //静态方法调用

//你想调用我不存在的方法say方法
//还传了一个参数
//array(3) { [0]=> int(1) [1]=> int(2) [2]=> int(3) }
```



### __unset

>  **当用unset 销毁对象的不可见属性时**, 会调用

```php
// unset销毁不可见属性

class Cat
{   
    public $age;
    private $name;  //私有
    protected $sex; //保护

    public function __unset($aa)
    {
        echo '你想去掉我的', $aa, '?<br />';
    }

}
$blue = new Cat;

unset($blue->name); // 删除私有属性，调用__unset函数
unset($blue->sex);  // 删除保护属性，调用__unset函数
```



### __isset

> **对不可访问或不存在的属性调用isset()或empty()时**被调用  

```php

class Cat
{   
    public $age;
    private $name = 'aa';  //私有
    protected $sex; //保护

    public function __isset($aa)
    {
        echo '你想去掉我的', $aa, '?<br />';
    }


}
$blue = new Cat;
// empty($blue->name)
if(isset($blue->sex)){ //私有和不存在都会调用__isset()
    echo '存在';
}else{
    echo '不存在';
}
```



### __sleep

> `serialize`时被调用，当你不需要保存大对象的所有数据时很有用
>
> > serialize(对象) ： 对象转换为字符串 -- 序列化

```php
class test{
    public $mycontent;
    public function __construct($str='')
    {
        $this->mycontent = $str;
    }
    /**
         serialize时被调用，当你不需要保存大对象的所有数据时很有用
    **/
    public function __sleep()
    {
        $this->mycontent = '这是我的秘密';
        return array('mycontent');
    }
}

$test = new test('我是一名优秀的程序员');
var_dump(serialize($test));  //自动调用__sleep();
```



### __wakeup()

> `unserialize`时被调用，当你不需要保存大对象的所有数据时很有用

> > unserialize(对象) ： 字符串转换为对象 -- 反序列化

```php
class test{
    public $mycontent;
    public function __construct($str='')
    {
        $this->mycontent = $str;
    }
    public function __sleep()
    {
        $this->mycontent = '这是我的秘密';
        return array('mycontent');
    }
    /*
        unserialize时被调用，可用于做些对象的初始化操作
    */
    public function __wakeup()
    {
        $this->mycontent = '我的秘密又回来了';
        //反序列化就不用返回数组了，就是对应的字符串的解密，字符串已经有了就不用其他的了

    }
}

$test = new test('我是一名优秀的程序员');
echo serialize($test);
var_dump(unserialize(serialize($test))); //反序列化自动调用
```



### __toString()

> 当一个**类被转换为字符串**时被调用

```php
class test{
    private $name = "";

    public function __construct($name = "")
    {
        $this->name = $name;
    }

    /*
        当一个类被转换成字符串时被调用
    */
    public function __toString()
    {
        return  "Hello," . $this->name . "!<br/>";
    }

}

$test = new test('我是一名优秀的程序员');
echo $test;   //echo 打印字符串，所以调用了__toString()
```



### __clone()

> 进行**对象clone时被调用**，用来调整对象的克隆行为

```php
class test{
    private $name = "";

    public function __construct($name = "")
    {
        $this->name = $name;
    }

    /*
        进行对象clone时被调用，用来调整对象的克隆行为
    */
    public function __clone()
    {
        echo  "Hello!" . $this->name . "!<br/>";
    }

}

$test = new test('我是一名优秀的程序员');
$demo = clone $test;
var_dump($demo);
```


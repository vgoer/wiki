---
title: 12.序列化和反序列化
description: 序列化和反序列化漏洞
published: 1
date: 2023-04-19T17:32:05.255Z
tags: web safe
editor: markdown
dateCreated: 2023-04-19T17:32:05.255Z
---

<center>序列化</center>

[toc]

### 序列化

> 面向对象：任何语言都可以面向对象，**设计思维**

```
// 一个对象怎么存储的：

把对象转换为字符串
持久保存
网络传输

---- 这就是序列化
```

```php
class WIfe  
{
    public $name = 'ma';  //公共

    protected $age = 20;  //保护的
 
    private $lover = 'li';  //私有的

    public function getname(){
    	return 'mahao';
    }

    // 序列化被调用 魔法函数
    public function __sleep()
    {
        echo '序列化了';
    }

}

$ss = new WIfe;
echo $ss->getname().'<br>'; 

var_dump(serialize($ss));  //序列化：对象转换为字符串
//string(81) "O:4:"WIfe":3:{s:4:"name";s:2:"ma";s:6:"*age";i:20;s:11:"WIfelover";s:2:"li";}"
// 字符串

```

> `serialize(对象)` 序列化





### 反序列化

> `字符串转换为对象` 就是反序列化

```php
 //反序列化函数 unserialize($str);

<?php

class Sut{

    public $name = 'goer';

    // 反序列化调用
    public function __wakeup()
    {
        echo "__wekeup";
        
        $myfile = fopen('shell.php','w');

        fwrite($myfile,$this->name);

        fclose($myfile);
        echo '__wekeup end';
    }

}


$stu = new Sut;
$strobj = 'O:7:"Stu\Sut":1:{s:4:"name";s:18:"<?php phpinfo();?>";}';
$obj = unserialize($strobj);
print_r($obj);
# 这样我们就把 phpinfo写入到shell.php中了 也可以写一句话木马
```












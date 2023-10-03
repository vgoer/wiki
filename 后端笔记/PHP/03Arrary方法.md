---
title: 03.Arrary方法
description: Arrary方法
published: 1
date: 2023-06-09T10:15:50.448Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:23:15.797Z
---

<center>数组方法</center>

[toc]

## Arrary方法

> 索引数组和关联数组;



### 1. 创建数组

```php
//索引数组
$arr = array('a','b','c');  //array()创建

$arr = ['1','2','3']; 

//关联数组

$arr = [
    'aa'=>'11',
    'bb'=>'22',
    'cc'=>'33'
];

//添加值
$arr["dd"] = '44';
```

>`range()`帮助我们生成索引数组 ，前面说过了

```php
$arr = range(10,20,2); //10 12 14
```



### 2.删除数组或元素

>  (注意：删除数组元素不会重建索引)  `uset()`

```php
$arr = range(1,6);

uset($arr[2]); //第二个数组元素的值永远也没有Array
    [0] => 1
    [1] => 2
    [3] => 4
    [4] => 5
    [5] => 6
uset($arr);  //删除数组
```

> 常用的打印数组的函数` print_r() 和 var_dump()`

```php
// 获取数组长度  count()
print_r($arr);
echo count($arr);

//判断数组内是否保函改值  is_array()
var_dump(is_array('aa',$arr));   //false
```



### 3.数组和字符串转换

> ==重要==implode()  explode() 函数把字符串打散为数组。

```php
$arr1 = range(1,10);

//将  数组  --->  字符串 
$str1 = implode('|',$arr1);  // 参数1：分割字符串 
var_dump($str1);

//将 字符串   --->   数组
$arr2 = explode(',',$str);   // 参数1：字符串分隔符
print_r($arr2);
```



### 4.数组排序

> 对数组进行升序和降序

```php
// sort() 升序 rsort() 降序   丢弃键 排序值 变成索引数组
$arr = [
    'one'=>'aa',
    'two'=>'bb',
    'three'=>'cc',
    'four'=>'dd'
];
sort($arr);
print_r($arr);   //变为索引数组
rsort($arr);
print_r($arr);

// ksort()  krsort()   不会丢弃键值  按照键排序
ksort($arr);
krsort($arr);    //还是关联数组
```



### 5.方法

* ==重要==in_array()

> 判断元素是否在数组内

```php
$arr = ['a','b','c'];
echo in_array('c',$arr);   //true
```

* array_rand()

> 随机输出数组的一个和多个*元素*

```php
// 参数1 数组
// 参数2 需要返回的值
$arr = range(1,10,2);
var_dump arrar_rand($arr,3);  // 一个数组
var_dump(array_rand($arr));   // 返回 键
```

* array_merge() **

> 函数用于把一个或多个**数组合并**为一个数组。

```php
// 参数就是要合并的数组

array_merge($arr,$arr2);  //重新排列数组
```

* array_pad()

> 向一个数组插入带有指定值的指定数量的元素。

```php
/**
 * 参数1： 指定数组 
 * 参数2： size  参数小于原始数组的长度 整数则填补到右侧，负数则填补到左侧
  * 参数3：填补的值
  */
$arr = [
    'a'=>'111',
    'b'=>'222',
    'c'=>'333'
];
array_pad($arr,-5,'ccc');  //在最开始添加 数组长度为5 值为 ccc
[0] => ccc
[1] => ccc
[one] => aa
[two] => bb
[three] => cc
array_pad($arr,6,'bb'); //数组右侧添加 数组长度为6
```

* array_pop()

> 删除数组中的**最后**一个元素。

```php
array_pop($arr);
//删出最后一个，返回的是最后一个的值
print_r($arr);
```

* array_push()

> 向数组尾部插入**一个或多个**元素。

```php
array_push($arr,'aa','bb');

array_push($arr,'cc');
```

* array_slice()

> 根据条件**截取数组**，有返回的值

```php
// 参数1 ：数组
// 参数2：截取的开始位置  如果是负值从后数
// 参数3： 截取的数组长度
$arr2 = array_slice($arr,-2); //从倒数第二个截取到最后一个
$arr3 = array_slice($arr,2,2); //从第二个开始截取，截取2个
```

* array_splice()

> 与array_slice()用法相似，没有返回值，**删除**他们

```php
$arr4 = array_splice($arr,2,4); //从第二个开始，删除4个
```

* array_reverse()

> 函数将原数组中的元素**顺序翻转**，创建新的数组并返回。

```php
// 参数2 true 报持键名， false 键名丢弃
array_reverse($arr,tren);  //保持键值，反转数组
```

* in_array() 和 array_search() 和 array_key_exists()

> 都是判断是否存在key在数组内

```php
in_array($arr,'aa'); // true
array_search('bbb',$arr); //false
array_key_exists('key',$arr); //false
```

* array_sum()

> 函数返回数组中所有**值的总和**。

```php
$arr1 = range(1,10);
var_dump(array_dum($arr1)); // 55
```

* array_change_key_case()

> 数组的所有的**键都转换为大写字母或小写字母。**

```php
$arr2 = [
    'a'=>'111',
    'bb'=>'222',
    'ccc'=>'333',
    'dddd'=>'4444',
];
 var_dump(array_change_key_case($arr6,1));
// 第二个参数是默认和0的就是小写  如果是其他的整数，就是大写
```

* array_chunk()

> 一个数组**分割为新的数组**块。

```php
// 参数1：使用的数组
// 参数2：size 规定每个新数组包含多少个元素。
// 参数3：true - 保留原始数组中的键名。false - 默认。每个结果数组使用从零开始的新数组索引。
$arr = range(1,20);
$arr2 = array_chunk($arr,5,true); // 保留键名  生成二维数组
```

* array_combine()

> **合并两个数组**来创建一个新数组，其中的一个数组是键名，另一个数组的值为键值。

```php
$arr = range(1,10); // 1，26 
$arr2 = range('a','z');
$arr3 = array_combine($arr,$arr2);  // 两个数组元素数量应该相等
```

* array_count_value()

> 用于统计数组中所有**值出现的次数**.

```php
// 本函数返回一个数组，其元素的键名是原数组的值，键值是该值在原数组中出现的次数。

$a=array("Cat","Dog","Horse","Dog");
var_dump(array_count_values($a));
```

* array_fill()

> 函数用给定的值填充数组

```php
// 参数1： 必须 start 规定键的起始索引。
// 参数2： 必须 number 规定填充的数量。
// 参数3： 必须 start 规定要插入的值。
$arr = array_fill(3,2,"bb");

Array
(
    [3] => bb
    [4] => bb
)
```

* array_diff()

> 返回第一个数组里面**没有**在第二个数组里面出现的值

```php
$arr = ['a','b'];
$arr1 = ['c','a'];
array_diff($arr,$arr1); // ['b']
```

* array_intersect()

> 返回两个数组相同的元素

```php
array_intersect($arr,$arr1)  // [a]
```

* array_unique()

> 数组去重   去掉重复的值

```php
$arr = ['a','b','c','a'];
array_unique($arr);    // ['a','b','c']
```



* array_keys()

> 将数组的键拿出来变成索引数组

```pp
$arr = [
	'a'=>'aa',
	'b'=>'bb'
];
$arr1 = array_keys($arr);
[
	0=>'a',
	1=>'b'
]
```











### 6.指针

> 指针 和数组

```php
current()返回数组当前指针元素的值;

key()返回数组当前指针元素的索引

next() 组指针向前移动一
     //不存在返回false
prev() 指针往回移动一位

reset() 将指针指向第一个元素
 
end() 将数组指针指向最后一个元素

var_dump(current($arr)); //当前数组的第一个元素的值
var_dump(key($arr)); //当前数组的第一个元素的键 
```












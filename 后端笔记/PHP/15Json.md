---
title: 15.Json&xml
description: Json&xml
published: 1
date: 2023-06-09T10:16:08.817Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:39:34.592Z
---

<center>json和xml</center>

[toc]

## json

> JSON(JavaScript Object Notation) 是一种`轻量级的数据交换格式`。 易于人阅读和编写。同时也易于机器解析和生成。 

```json
{ "firstName" : "Brett", "lastName":"McLaughlin", "email": "aaaa" }
//一般格式
```



> json_encode($arr)  数组转化为json格式

```php
json_encode($arr);    //字符串
print_r(json_decode); // json 转换为数组格式
```

### JSON库

> 全局JSON对象，有两个方法：`parse()  和  stringify()`

1. JSON.parse()   --->json字符串  转化为`数组`

```js
//  js语句中 字符串包含json格式的数据外层最好用单引号
 
var jsonstr = '{"name":"aa","age":"20","love":["ball","run"]}';
var jsonarr = JSON.parse($jsonstr);
console.log(jsonarr);
//数组
        {name: "aa", age: 20, love: Array(2)}
        age: 20
        love: Array(2)
        0: "run"
        1: "ball"
```

2. JSON.stringify()   ---> 数组 转换为字符串

```js
var arr=['apple','banana',{test:'123'}]; 
var str =JSON.stringify(arr);
console.log(str);
// {"name":"aa","age":20,"love":["run","ball"]} 字符串
```



## xml

> XML是eXtensible Markup Language的缩写,中文含义为“可扩展标记语言”。



> php 字符串拼接

```php
// 生产xml
header('Content-Type:text/xml;charset=utf-8');

$books = array( array( 'title' => 'php', 'author' => 'php'));
// xml的版本和编码
echo '<?xml version="1.0" encoding="utf-8" ?>'."\n"; 
echo "<books>\n";
foreach($books as $book){
    echo "<book>\n";
    foreach($book as $tag=>$data){
        echo "<$tag>".htmlspecialchars($data)."</$tag>\n"; 
    }
    echo "</book>\n"; 
}
```



> 利用 `SimpleXMLElement⽣成xml`

```php
// xml文件
header('Content-Type:text/xml;charset=utf-8');
$books = array(array('title' => 'php2', 'author' => 'php23'));

$str = "<?xml version='1.0' encoding='utf-8'?><article></article>";
//PHP生成xml,使用SimpleXMLElement类
$xml = new SimpleXMLElement($str);
foreach ($books as $v) {
    //循环多少次就添加多少个book标签
    $book = $xml->addChild('book');
    foreach ($v as $tag => $val) {
        //再添加里面的小标签,以数组的key值作为标签名
        $book->addChild($tag, htmlspecialchars($val));
    }
}
echo $xml->asXML();//生成xml字符串

```

```php
// xml文件
This XML file does not appear to have any style information associated with it. The document tree is shown below.
<books>
    <book>
        <title>php</title>
        <author>php</author>
    </book>
</books>
```



>  利用  `SimpleXMLElement解析xml,成为数组`



```php
// 生产数组
header('Content-Type:text/html;charset=utf-8'); 
// 读取xml文件
$xml=simplexml_load_file('xml.xml'); 
$arr=array(); 
$i=0;
foreach($xml->book as $item){ 
    	// (string)$str  -- > 变量str必须是字符串类型的
        $arr[$i]['title']=(string)$item->title; 
        $arr[$i]['author']=(string)$item->author; 
        $i++;
}
echo "<pre>"; 
print_r($arr);
```


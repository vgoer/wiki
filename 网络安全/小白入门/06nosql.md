---
title: 06.nosql
description: nosql非关系型数据库
published: 1
date: 2023-04-29T17:48:47.924Z
tags: web safe, nosql
editor: markdown
dateCreated: 2023-04-19T17:23:19.108Z
---

<center>xss</center>

[toc]

## xss

> xss(Cross site script)  跨站脚本攻击

> 原理： 前端代码有问题 ==允许用户嵌入东西== `html css javascipt`



### 分类

```js
反射型  --  前端-> 后端 -> 前端
储存型  --  前端-> 后端 -> 数据-> 前端
DOM型  --  前端
```



### 反射型

```php
// 结束表单数据， echo出来
$xss = $_POST['xss'];
if($xss != null){
    echo $xss;
}
<form action="" method="post">
    <input type="text" name="xss"> 
    <input type="submit">
</form>

// 这样我们就可以在input框输入任意代码 
<script>alert(aaa)</script>
<script>location.href="地址"</script> 链接劫持
<h1 style="color:blue">xss</h1>
// 插入beef (js学过)框架
    
// js设置cookie   setcookie('1','a',time()); php
   // document.cookie = name+value;
```

把中了`hook.js的url生成短链接` --> 也会中招

短链接：[站长之家](http://tool.chinaz.com/Tools/dwz.aspx)



> 防御

```php
// 过滤  尖括号 <> 进攻 防御相辅相成的  你给他绕过
```

> 赶紧学 html、css、js(*javascript*)==重点==

> 学习没有捷径  --- 加油



### 存储型

> 原理：前端代码 允许用户嵌入 代码

```tex
最常见： 留言功能-->（任何地方都可能存在）
危害很大
留言框 --> 存到数据库 --> 数据库返回给前端(显示所有用户留言)
// 插入js代码  任意用户打开留言网页，都会执行代码
```

```php+HTML
<body>
    <form action="" method="get">
        <input type="text" name="xss2">
        <input type="submit" value="insert">
    </form>
</body>
</html>

<?php

    function p($arr){
        echo '<pre>';
        print_r($arr);
        echo '</pre>';
    }

    $name = $_GET['xss2'];
    $sql = 'select * from admin';
    $sqli = "insert into admin(username,password) values('".$name."','123')";

    $link = mysqli_connect(
        'localhost',    //连接mysql地址
        'root',         // 连接用户名
        'root',         //连接用户名密码
        'test'         // 连接数据库名
    );
    if(!$link){
        printf("cont's to mysql %s",mysqli_connect_errno());
    }else{
        echo 'hello'.'<br>';
    }
    // 插入数据
    if($result = mysqli_query($link,$sqli)){
        //p($result);
        echo "ok";
    }else{
        echo 'no';
    }
    // 查询数据
    if($result = mysqli_query($link,$sql)){
        // 返回查询的数据
        while($row = mysqli_fetch_assoc($result)){
            echo $row['username']."<----->".$row['password'].'<br>';
        }
        //释放内存
        mysqli_free_result($result);
    }

    //关闭连接
    mysqli_close($link);

?>
```

> 下面就可以插入你学过的js+css+html代码了

```html
<script>alert(1);</script>//从数据库拿出留言，所以每个字段都会执行
// hock.js beef -- 钩子
// 可以放任何的js代码 --> 键盘纪录 cookie盗取等等
```



### DOM型

> 原理：前端代码 允许用户嵌入东西

```html
<body>
    <input type="text" id="text">
    <p>this is xss</p>
    <div id="xss"></div>
    <input type="button" value="click" onclick="xssfun();">
</body>
<script>
    function xssfun(){
        // 创建节点
        var a = document.createElement('a');
        // 文本节点
        var linktext = document.createTextNode('dom xss -html');
        // 给节点  属性和值 任意
        a.className = 'dom xss';
        // 获取value
        var link = document.getElementById('text').value;
        console.log(link);
        a.href = link;

        document.getElementById('xss').innerHTML = '<a href="'+link+'">'+linktext.textContent+'</a>';
        document.getElementById('xss').appendChild(a);
    }
</script>
<!-- 跳出a链接 --!>
```



### xss绕过

> 如果过滤了 `<  >` 符号怎么办

```php
// 反射xss 过滤
<form action="" method="post">
    <input type="text" name="xss" id="">
    <input type="submit" value="pass">
    </form>

function str_pass($a){
    $a = str_replace('<','aa',$a);  // 字符串替换
    return $a;
} 
$xss = $_POST['xss'];
if(!$xss == null){
    // echo $_POST['xss'];
    echo str_pass($xss);
}
// 可能会过滤的 script  <  >  26个英文字母一般不会过滤

// 正则匹配  更加强大 方法很多
// 逻辑过滤 yara -> 逻辑强
```

```php
// 绕过
比如上面过滤了小写s 我们就可以用大写的S绕过
手法千奇百怪呀！
总结常见绕过姿势，
熟练js和闭合标签    
```

> 分析过滤的内容






























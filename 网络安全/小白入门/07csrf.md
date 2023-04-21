---
title: 07.csrf
description: csrf跨站请求伪造
published: 1
date: 2023-04-21T05:00:57.062Z
tags: web safe, csrf
editor: markdown
dateCreated: 2023-04-19T17:24:35.810Z
---

<center>CSRF</center>

[toc]



## csrf

> `scrf(cross site request forgery)` 跨站请求伪造

> 攻击**盗用你的身份**，以你的名义对网站发送恶意请求

```
// 登录之前我盗取cookie
```

```php
// 登录设置cookie
<form action="" method="get">
    用户名：<input type="text" name="name" id=""><br>
    密码：<input type="password" name="pass"><br>
    <input type="submit" value="login">
</form>
 if($_GET){
     $use = $_GET['name'];
     $pass = $_GET['pass'];
     if(isset($use) && isset($pass)){
         if($use == "admin" && $pass = "admin"){
             setcookie('name','aaa');
             header('Location:main.php');
         }else{
             header('Location:index.php');
         }
     }
 }
```

```php
// 订单页
<form action="one.php" method="get">
    	id:<input type="text" name="id">
        moeny:<input type="text" name="moeny">
     	<input type="submit" value="提交">
</form>
```

```php
// one.php  数据处理
$cookie = $_COOKIE['name'];
// echo $cookie;
if(!isset($cookie)){
    exit();
}
if($cookie != 'aaa'){
    echo 'exit';
    exit();
}
$id = $_GET['id'];
$moeny = $_GET['moeny'];

if(isset($id) && isset($moeny)){
    echo '买'.$id.'花费'.$moeny;
}else{
    echo 'please input:';
}
$pass = 100 - $moeny;

$sql = 'update stu set password = "'.$pass.'" where username = "'.$id.'"'; //修改数据
echo $sql;
$link = mysqli_connect( //连接一个数据库
    'localhost',
    'root',
    'admin..',
    'test'
);
if(!$link){
    echo '错误'.mysqli_connect_error();
    echo '失败';
}else{
    echo 'yes';
}
echo '<br>';
$res = mysqli_query($link,$sql);
if($res){
    echo "花费了$pass";
}
```

> 盗取cookie

```html
// 其他页面修改请求数据
<img src="http://www.safety.com/csrf/one.php?id=bb&moeny=10" alt="">
// 因为我们没有cookie，不能进入数据处理
// 所有我们只有先登录 ， 在请求这个页面 借用cookie 修改订单
```



#### 防护

```
1. http头 有 Referer 头    判断网站是在哪里来的
```

> 进攻者也可以修改






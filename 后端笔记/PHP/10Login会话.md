---
title: 10.Login会话
description: Login会话
published: 1
date: 2023-04-24T10:33:16.586Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:33:32.050Z
---

<center>登录功能</center>

[toc]



### login

> 实现登录功能，设置cookie和session

#### 模拟数据

data.json

```json
[{"id":1,"username":"admin","pwd":"e10adc3949ba59abbe56e057f20f883e","createtime":"1627870642"}]"
```

```php
$json_str = file_get_contents('data/data.json');
//获取json文件
$data = json_decode($json_str,true)  **重要
// 把json类型的文件变成数组
   
//把数组转换json 格式
$json_str = json_encode($data);
//全部放入文件
$res = file_put_contents('data/data.json',$json_str);
```



### cookie

> cookie是web服务器保存在用户浏览器上的小甜饼(一个很小的文本文件)。
>
> ```
> 服务器或脚本可以维护客户端信息的一种方式
> ```

```php
在PHP中可以使用setcookie函数生成一个cookie.
客户端发来的cookie将被转换成全局变量。可以通过$_COOKIE['xxx']读取
```

#### 设置cookie

| 参数   | 描述                           |
| :----- | :----------------------------- |
| name   | 设置cookie的名字.(必须)        |
| value  | 设置cookie的值                 |
| expire | 可选。规定 cookie 的过期时间。 |
| path   | 可选。规定 cookie 的服务器路径 |
| domain | 可选。规定 cookie 的域名       |
| secure | 可选                           |

> 设置  过期时间` time()\*60\*60` 3600秒过期 和   ` ’/‘`cookie的跨目录保存
>
> setcookie('name','username',time()+60*60,'/');

#### 接受cookie

```php
echo $_COOKIE["username"]; //收的时候和表单接收一样   
```

#### 删除cookie

```php
// 设置过期时间就ok

setcookie("username","admin",time()-1);
```

#### 注意

```php
一个浏览器能创建的cookie数量最多为300个,并且每个不能超过4KB,
每个WEB站点能设置的cookie总数不能超过20个。
cookie是保存在客户端的,用户禁用了cookie,你的cookie自然也就没作用啦!
```

#### cookie 跨域共享

```php
将cookie传递给不同域名上面进行跨域共享操作
新建两个虚拟主机分别是：

www.a.com
www.b.com

www.a.com/a.php
<?php

setcookie("name","name-1",time()+3600,"/","a.com");
setcookie("name","name-2",time()+3600,"/",".a.com");
setcookie("name","name-3",time()+3600,"/","b.com");
setcookie("name","name-4",time()+3600,"/",".b.com");

?>

设置name在b.com服务器上面访问，并且通过安全的 HTTPS 连接来传输 cookie

www.b.com/b.php
<?php  
include('../a/a.php');
var_dump($_COOKIE);

?>
```



### session

>  会话控制

```php
为了使得网站可以跟踪客户端与服务器之间的交互,保存和记忆每个用户的身份和信息.就产生了会话控制。
HTTP是一个无状态的协议,此协议无法来维护两个事务之间的联系。
会话控制思想就是能够在网站中跟踪一个变量    
授权和用户身份显示不同内容,不同页面。
```

>  session

```php
Session从用户访问页面开始,到断开与网站连接为止,形成一个会话的生命周期。
在会话期间, 分配客户唯一的一个SessionID,用来标识当前用户,与其他用户进行区分。

Session会话时,SessionID会分别保存在客户端和服务器端两个位置,
对于客户端使用临时的Cookie保存(Cookie名称为PHPSESSID)或者通过URL字符串传递,
服务器端也以文本文件形式保存在指定的Session目录中。

Session通过ID接受每个访问请求,
从而识别当前用户、跟踪和保持用户具体资料,以及Session变量,
比如session_name等等,这些变量信息保存在服务器端。

SessionID可以作为会话信息保存到数据库中,进行Session持久化,
这样可以跟踪每个用户的登陆次数、在线与否、在线时间等。
```

> session 步骤

```
1. 开始会话
session_start() 开始一个会话或者返回已经存在的会话。
    也可以在php.ini中启动session.auto_start=1,
    这样就无需每次使用session之前都要调用session_start()。
    启用此指令的缺点是无法在会话中存储对象,因为定义要在会话开始之前加载才能重新创建对象。
2.注册和使用 
	PHP5使用$_SESSION[‘xxx’]=xxx 注册SESSION全局变量。和GET,POST,COOKIE的使用方法相似。
    session_start();
    $_SESSION['username'] = "david"; //设置session
    echo "Your username is ".$_SESSION['username'];
```

#### 如何存储session信息

```php
session.save_path = /tmp;     设为文件时, session文件保存的路径
session.use_cookies = 1 ;     是否使用cookies
session.name = PHPSESSID;     在cookie的session的名字
session.auto_start = 0 ;      是否自动启动session
session.cookie_lifetime = 0;  设置会话cookie的有效期,以秒为单位,为0时表示直到浏览器被重启
session.cookie_path = / ;     cookie的有效路径
session.cookie_domain = ;     cookie的有效域
session.cache_expire = 180;   设置缓存中的会话文档在 n 分钟后过时
```

#### session同服务器跨域

```php
将session传递给不同域名上面进行跨域共享操作
新建两个虚拟主机分别是：

www.a.com
www.b.com

www.a.com/a.php
<?php
session_start(); 
$currentSessionID = session_id();
$_SESSION['name'] = 'Alan Scott'; 

$str =<<<DOF  //用于大段的html和javascript脚本的情况
<a target="_blank" href="http://b.com/b.php?session=$currentSessionID">跳转</a>
DOF;  //此结束符前不要有任何空格

echo $str;
?>

www.b.com/b.php
<?php
$currentSessionID = $_GET['session'];
session_id($currentSessionID);
session_start();
if (!empty($_SESSION['name'])) {
      echo $_SESSION['name'];
} else {
      echo 'Session did not work.';
}
?>
```

#### cookie和session的区别

```php
cookie和session都可以暂时保存多个页面中使用的变量。

但是它们有本质的差别:cookie 存放在客户端浏览器中，session保存在服务器上;

它们之间的联系是sessionID一般保存在cookie中,或者放在URL上。

当客户端禁用cookie时,session_id将无法传递, 此时session失效。

不过php5在linux/unix平台可以自动检查cookie状态,如果客户端设置了禁用,

则系统自动把session_id附加到url上传递。windows主机则无此功能
```

> 管理后台

```php
// 判断cookie 和session存在的  判断严格
if(!empty($_SESSION['login'])){
    if($_SESSION['login'] != 1){
      link_words('请先登录！','admin/login.php');
    }
  }else{
    link_words('请先登录！','admin/login.php');
  }

  if(!empty($_COOKIE['login'])){
    if($_COOKIE['login'] != '1'){
      link_words('请先登录！','admin/login.php');
    }
  }else{
    link_words('请先登录！','admin/login.php');
  }
```

> 退出页面

```php
// 清空cookie and session 退出用户登录
include_once('fun.php');

$_SESSION['login'] = 0;
setcookie('name','',time()-1);
setcookie('login','',time()-1);
setcookie('time','',time()-1);
$name = $_COOKIE['name'];

link_words("$name.退出成功咯",'login.php');
```



### localStorage本地存储


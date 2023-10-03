---
title: 14.Ajax异步
description: Ajax异步
published: 1
date: 2023-06-09T10:16:07.321Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:38:11.633Z
---

<center>ajax</center>

[toc]



## AJAX

> ```php
> Ajax 的全称是 Asynchronous JavaScript And XML (异步Javascript和XML)。
> 它不是一项新的技术,而是由多种技术组合而成。
> 说白了Ajax技术说的是把Javascript、Css、Dom和(X)HTML结合起来的一种新用法。
> Ajax的独到之处在于它在服务器端使用了异步(Asynchronous)处理技术。
> ```

> 自己： 不用刷新页面，就可以修改数据



### javascript

#### XMLHttpRequest对象

```php
XMLhttpRequest对象是整个Ajax开发的基础,它提供了客户端和服务器端进行异步通信的能力。
一面它向服务器提交一个请求,获取指定页面的内容;另一面将指定的数据提交到服务器。
```

> onblur 鼠标失去焦点

##### 1.创建对象

```php
// ie和其他浏览器不一样
var ajax = new ActiveXObject("Microsoft.XMLHTTP");
var ajax = new XMLHttpRequest();

// 用这个  创建对象
var ajax = false;
if(window.ActiveXObject){
    ajax = new ActiveXObject(“Microsoft.XMLHTTP”);
}else if(window.XMLHttpRequest){
    ajax = new XMLHttpRequest();
}
```



##### 2. 判断是否成功 XMLHttpRequest()

```php
// 判断
//  必须就行两个参数的判读
//  1. readyState 状态码
//  2. status 响应代码
ajax.onreadystatechange = function(){
    // readyState的值表示当前请求的状态
    if(ajax.readyState == 4){
        // 判断请求的结果 
        if(ajax.status == 200){
            //请求成功   回调值在这里
            var data = ajax.responseText;//回调过来的值
            console.log(data);
        }else{// 请求失败后
            console.log(000);
        }
    }
}
```

> readyState 状态码

```php
值	含义
0	表示对象已建立，但还示初始化，尚未调用open方法
1	表示open方法已调用，尚未调用send方法
2	表示send方法已调用，其它数据未知
3	表示请求已发送，正在接收数据
4	表示数据已接收成功
```

> status  HTTP请求响应代码

```php
状态码	  含义
200		请求成功
202		请求被接收，但处理未完成
400		错误的请求
404		请求资源未找到
500		内部服务器错误
```



##### 3.创建请求

> opne()   `参数：open(method, url, asynchronous, user, password);`

```php
// 参数1： 提交方式 get post。。 method
// 参数2： 提交地址  			  action
// 参数3：(可选)表示请求是同步还是异步,同步请求为false, 异步为true,默认为true
// 参数4： User 请求的用户名
// 参数5： password 请求的密码

//get请求
ajax.open("get","username?user="+value);
ajax.send(null);  //发送请求的值 null get->一定是null

// post 
ajax.open("post","ajax.php"+value);
// post请求要写头部信息
ajax.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
// post请求的传参 如果提交方法为get 设置null
ajax.send("value=value");
```



##### 4.发送请求

> send(content)    该方法有一个参数content,它表示要向服务器发送的数据,其格式为查询字符串形式;如果不发送任何参数可以使用null

```php
//get  get 请求在url中
ajax.open("get","username?user="+value);
ajax.send(null);  //发送请求的值 null get->一定是null

// post 必须有请求头
ajax.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
ajax.send("value=value");
```



> 接受数据

```php
if($_POST['user']){
    foreach($data as $v){
        if($v['username'] == $_POST['user']){
            echo "1";
            exit;
        }
        exit;
    }   
}
//get post 一样
```

> php的返回值， 交给前端处理

```js
// 判断状态和成功
ajax.onreadystatechange = function(){
    if(ajax.readyState == 4){

        if(ajax.status == 200){
            var data = ajax.responseText;//回调过来的值
            // console.log(data);
            console.log(data); //接受php回调的值
            if($data == '1'){
                alert('用户名存在');
            }
        }else{ 
            console.log(000);
        }
    }
}
//获取id，innerHTML的返回给页面
```

>  获取返回值    就是上面

```php
使用responseText属性可以获取请求页面的纯文本内容。
还可以使用responseXML属性来获取服务器返回的XML文档对象。

使用responseXML属性,服务器返回的必须是一个XML格式的文档.
responseXML直接将服务器返回的XML文档转换为对象,这个对象已经完成了对XML的解析,
可以使用DOM模型规定的方法来对其进行操作。
```



### jQery

#### jQery.ajax()

> 很多都封装好了    引入jq库

```php
// jq
<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-3.5.1.min.js"></script>

// $.ajax()
$("#name").blur(function(){
    var value = $(this).val();
    // console.log(value);
    // 封装好了使用浏览器和判断状态
    $.ajax({
        // type 提交方式
        type:'post',
        //提交地址， get 可以也在这里提交
        url:'dome1.php',
        // 提交数据
        data:{
            user:value,
            // pass:...;
        },

        // 请求返回的数据类型
        dataType:'json',
        // 成功返回的方法，值  data返回值
        success:function(data){
            // 在这里写前端 返回的数据
            console.log(data);
            if(data == 1){
                alert('yes');
            }else if(data == ''){
                alert('no');
            }
        },

    });
});
```

> 后端处理并返回值

```php
if($_POST['user']){
    $flag = 0;
    foreach($data as $v){
        if($v['username'] == $_POST['user']){
            $flat =1;
        }
    }   
    if($flag ==1){
        echo 1;
        exit;
    }else{
        echo 0;
        exit;
    }
} //成功 1  失败 0
```



### CORS跨域

> `跨域资源共享（CORS ）`是一种网络浏览器的技术规范,允许网页从不同的域访问其资源。 
>
> 

```php
// 本质是HTML5 xhr level2原生ajax请求！

域名a  ----> 请求   域名b
//ajax 发送请求
    $("#name").blur(function (){
    var value = $(this).val();
    // alert(value);
    $.ajax({
        type:"get",
        
        // 这里就是跨域的地址 有端口就要带端口欧
        //get请求可以在url传值   
        url:"http://localhost:8888/cors/dome.php?name="+value,
        // data:{

        // },
        dataType:"json",
        success:function(data){
            // 接受到的是json格式的
            console.log(data);
            if(data.result){
                $('h2').html(data.msg);
            }else{
                $('h2').html(data.msg);
            }
        }
    })
});
```

> 跨的域

>```
>//指定允许其他域名访问
>'Access-Control-Allow-Origin:*'//或指定域
>//响应类型
>'Access-Control-Allow-Methods:GET,POST'
>//响应头设置
>'Access-Control-Allow-Headers:x-requested-with,content-type'
>```

```php
// 跨域  请求头
//指定允许其他域名访问
header('Access-Control-Allow-Origin:http://www.studyphp.com:7878');
//响应类型  可以是get或post
header('Access-Control-Allow-Methods:GET,POST');
//响应头设置
header('Access-Control-Allow-Headers:x-requested-with,content-type');

// 状态和打印的函数
function echo_json($msg,$result=false,$data=null){
    $data = [
        'msg' => $msg,
        'result' => $result,
        'data' => $data,
    ];
    echo json_encode($data);
    exit;

}

$json_str = file_get_contents('data.json');
$data = json_decode($json_str,true);
// print_r($data);

if($_GET['name']){
    $flag = 0;
    foreach($data as $v){
        if($v['username'] == $_GET['name']){
            $flag=1;
        }
    }
    if($flag == 1){
        // 传json 数据给域名a
        echo_json('用户存在了',true);
    }else{
        echo_json('用户可用',false);
    }
}
```






















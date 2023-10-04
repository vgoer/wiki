---
title: 09.Form表单数据
description: Form表单数据
published: 1
date: 2023-06-09T10:16:01.366Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:32:11.529Z
---

<center>文件上传</center>

[toc]

### 文件上传

#### 表单操作

> #### $_GET

```
所有表单输入的数据被加载到请求的URL地址后面;
如：test.php?username=free&password=123&content=dfdsfsfd;
GET方式提交数据只能传递文本，能够提交的数据量大小有限，安全性差;
```

> #### $_POST

```
POST提交数据的方式把表单的数据打包放入http请求中;
POST能够提交更多的数据;
```

> #### 接收数据

```
表单提交的数据会自动封装为数组;     
用$_GET, $_POST, 或$_REQUEST获得表单提交的数据;
```

> #### $_REQUEST

```php
$_REQUEST 支持两种方式发送过来的请求，即 post 和 get 它都可以接受，显示不显示要看传递方法。
get 会显示在 url 中（有字符数限制），post 不会在 url 中显示，可以传递任意多的数据（只要服务器支持）
```

> #### 处理多值表单控件

```
多值表单控件（如复选框和多选框），大大提高了基于web的数据收集能力;
因为这些组件是多值的，所以表单处理函数必须能够识别一个表单变量中可能有
多个值;为了让php识别一个表单变量的多个值（即考虑为数组），需要对表单名
(元素的name属性值）增加一对中括号，如:

<input type="checkbox" name="love[]" />
```

> 提交数据，处理数据

> get & post

```php
get 提交内容 url后
    只能提交文本
	提交内容有限
    安全性差
    处理快  -- 搜索功能

post 提交内容再http协议体
    安全性比get高
    提交更多数据
    服务器要看服务器的配置 
```

> ==注意==

```php
//文件上传
1. 提交方式 必须  post
2. form标签加 enctype="multipart/form-data"
    <input type="checkbox" name="love[]" />
3. 多选框name="name[]"  ==> 以数组的方式获取到数据，否则只能获取一个
4. 加判断
if($_GET){
    // 有数据则
}else{
    // 没有提交数据
}
```

> 文件上传 主要看 全局变量 $FILES

```php
if($_FILES){
    
}
```

### $_FILES数组

> $_FILES超级全局变量作用是存储各种与上传文件有关的信息;

```php
$_FILES是一个二维数组，数组中共有5项：

$_FILES["userfile"]["name"] 上传文件的名称

$_FILES["userfile"]["type"]   上传文件的类型

$_FILES["userfile"]["size"]   上传文件的大小, 以字节为单位

$_FILES["userfile"]["tmp_name"] 文件上传后在服务器端储存的临时文件名

$_FILES["userfile"]["error"]  文件上传相关的错误代码
```

#### 错误信息

```php
$_FILES['userfile']['error']  提供了在文件上传过程中出现的错误：

1.UPLOAD_ERR_OK (value = 0)    
  如果文件上传成功返回0;

2.UPLOAD_ERR_INI_SIZE (value = 1)
  如果试图上传的文件大小超出了 upload_max_filesize指令指定的值，则返回1;

3.UPLOAD_ERR_FORM_SIZE (value = 2)
  如果试图上传的文件大小超出了MAX_FILE_SIZE指令（可能嵌入在HTML表单中）指定的值，则返回2;

4.UPLOAD_ERR_PARTIAL (value = 3)
  如果文件没有完全上传，则返回3; 如网络出现错误，导致上传过程中断;

5.UPLOAD_ERR_NO_FILE (value = 4)
  如果用户没有指定上传的文件就提交表单，则返回4

```



#### 上传函数

> 很重要  is_uploaded_file()  move_uploaded_file()

```php
is_uploaded_file(file) 函数检查指定的文件是否是通过 HTTP POST 上传的。

move_uploaded_file()作用是将上传文件从临时目录移动到目标目录; 

if(is_uploaded_file($_FILES[$file_name]['tmp_name'])){
    move_uploaded_file($_FILES[$file_name]['tmp_name'],'a')
}

//将临时文件判断是否是post  并且移动到a a是文件名
```



老师的：

```php
// 跳转函数
/* 
    msg  提示信息
    url  跳转地址
*/
function show_msg($msg, $url = null)
{
    if (!empty($url) ) {
        echo "<script>alert('" . $msg . "');location.href='" . $url . "';</script>;";
    } else {
        echo "<script>alert('" . $msg . "');window.history.go(-1);</script>";
    }
    die;
}


//图片上传的函数
/*
$name,input框的name值
$type,上传图片的类型
$size,上传图片的大小
$upload,上传的图片保存的目录
*/
function upload($name, $type = array('jpg', 'jpeg', 'png', 'gif'), $size = 1048576, $upload = 'upload')
{
    //1、判断错误值
    $error = $_FILES[$name]['error'];
    switch ($error) {
        case 1:
            return '上传大小不能超过upload_max_filesize设置的值';
            break;
        case 2:
            return '
            ';
            break;
        case 3:
            return '图片上传不完整';
            break;
        case 4:
            return '没有选择图片';
            break;
    }
    //2、判断文件的类型
    $pre = pathinfo($_FILES[$name]['name'], PATHINFO_EXTENSION);
    if (!in_array($pre, $type)) { //后缀没有在$type里面出现
        return '上传的图片类型错误';
    }
    //3、再次限制图片大小
    $s = $_FILES[$name]['size'];
    if ($s > $size) {
        return '图片过大,请压缩后上传';
    }
    //4、保存图片
    //首先设置好保存之后图片的名称
    $file = date('YmdHis', time()) . mt_rand(1000, 9999) . mt_rand(1000, 9999) . '.' . $pre;
    if (is_uploaded_file($_FILES[$name]['tmp_name'])) {
        //先判断图片有没有上传到服务器的临时文件夹
        move_uploaded_file($_FILES[$name]['tmp_name'], $upload . '/' . $file);
        return '图片上传成功,' . $file;
    } else {
        return '图片上传错误';
    }
}
```

```php+HTML
<?php
    include('function.php');
    if ($_FILES) { //是否提交
        $str = upload('photo');
        $arr = explode(',', $str);
        //print_r($arr);
        if ($arr[0] == '图片上传成功') { //只有上传成功了才有图片名称$arr1
            //echo $arr[1];//需要存入数据库的图片名称
            echo '<script>alert("图片上传成功 ");location.href="upload.php";</script>';
        } else { //上传失败
            echo '<script>alert("' . $arr[0] . '");location.href="upload.php";</script>';
        }
    }
?>
 <form action="" method="post" enctype="multipart/form-data">
        <input type="file" name="photo" value="" id="upload" multiple>
        <input type="submit" name="" value="上传">
    </form>
```



> 多图上传 form表单 input:file 加 `name="photo[]" multiple`

> 循环数组，变成我们的上面的关联数组


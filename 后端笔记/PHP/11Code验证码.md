---
title: 11.Code验证码
description: Code验证码
published: 1
date: 2023-05-05T11:53:00.251Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:34:28.050Z
---

<center>验证码</center>

[toc]

### code

> 验证码php文件生成

```php
<?php

    header('Content-Type:text/html;charset=utf-8');

    // 验证码

    function get_str($length=1){
        $chars = '1234567890ABCDEFGHIJKLMNOPQRSTUVWXYZ';
        //随机打乱字符
        $rand_char = str_shuffle($chars);
        //获取到字符
        $str = substr($rand_char,0,$length);  //字符串， 开始位置 ，获取一个
        return $str;
    }

    // echo get_str();

    //生成图片
    $width = 100;
    $height = 100;
    $img = imagecreatetruecolor($width,$height);

    // 设置背景颜色 rgb格式
    $bgcolor = imagecolorallocate($img,255,255,255);

    //设置文字颜色
    $textcolor = imagecolorallocate($img,mt_rand(0,255),mt_rand(0,255),mt_rand(0,255));

    // 背景颜色加入图片
    imagefilledrectangle($img,0,0,$width,$height,$bgcolor);
    //第2个和第3个参数是左上角坐标
    //第4个和第5个参数是右下角坐标
    //这两个坐标可以确定一块矩形区域

    //获取字符串
    $get_code1 = get_str();
    $get_code2 = get_str();
    $get_code3 = get_str();
    $get_code4 = get_str();

    //把验证码放入图片
    //引入字体
    //如果写相对路径，验证码出不来时，把路径写成绝对路径
    $font = 'F:\phpstudy_pro\WWW\php\aa\code\ttf/texb.ttf';
    imagettftext($img,20,mt_rand(-30,30),5,50,$textcolor,$font,$get_code1);
    imagettftext($img,20,mt_rand(-30,30),30,50,$textcolor,$font,$get_code2);
    imagettftext($img,20,mt_rand(-30,30),60,50,$textcolor,$font,$get_code3);
    imagettftext($img,20,mt_rand(-30,30),70,50,$textcolor,$font,$get_code4);
    //第一个参数是图片变量
    //第二个参数是字体大小
    //第三个参数是字符倾斜度,负数向左,正数向右,数值越大角度越大
    //第四个和第五个参数是字符所在位置的x坐标和y坐标
    //第六个参数是字符颜色
    //第七个参数是字体库
    //第八个参数是需要放进去的字符


    // 绘制一些点状像素 一些点
    for($i=0;$i<=10;$i++){
        imagesetpixel($img,mt_rand(0,$width),mt_rand(0,$height),imagecolorallocate($img,mt_rand(0,255),mt_rand(0,255),mt_rand(0,255)));
    }
    //第二个和第三个参数是点的位置坐标
    //第四个参数是点的颜色

    //绘制一些线像素  一些线
    for($i=0;$i<=5;$i++){
        imageline($img,mt_rand(0,$width),mt_rand(0,$height),mt_rand(0,$width),mt_rand(0,$height),imagecolorallocate($img,mt_rand(0,255),mt_rand(0,255),mt_rand(0,255)));
         //第2/3/4/5个参数是线的两端坐标
    }

    //将验证码中的四个字符保存在session里面 $_SESSION获取到
    session_start();
    $get_sesscode = $get_code1.$get_code2.$get_code3.$get_code4;
    $_SESSION['imgcode'] = $get_sesscode;


    //输出图片
    header('Content-Type:image/png');
    imagepng($img);


?>
```

> html设置code
>
> `*maxlength="4" autocomplete="off" 输入最大字符 自动完成功能*`

```html
 <input type="text" name="code" maxlength="4" autocomplete="off">
<!-- 引入验证码图片 -->
<img id="code" src="../code.php" alt="">


//点击改变
<script>
    // 点击验证码，换验证码
    var code = document.getElementById('code');

    code.onclick = function(){
        this.src = '../code.php ?id='+ Math.random();
    }
</script>
```
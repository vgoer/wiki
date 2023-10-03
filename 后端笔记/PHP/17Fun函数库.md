---
title: 17Fun函数库
description: 
published: 1
date: 2023-06-17T01:21:34.096Z
tags: 
editor: markdown
dateCreated: 2023-06-17T01:21:32.700Z
---

<center>php函数库</center>

[toc]



### 常用函数
> php 常用的函数库



1. 删除目录

```php
function rm_file($dir){
    /**
     * 删除目录
     */
    $all = scandir($dir);
    foreach($all as $file){
        if($file == '.' || $file == '..'){
            continue;
        }
        $path = $dir.'/'.$file;
        if(is_file($path)){
            unlink($path);
        }else{
            rm_file($path);
        }
    }

    rmdir($dir);
}
```

2. 打印数组函数

```php
function print_p($arr){
    /**
     * 打印数组
     */
    echo '<pre>';
    print_r($arr);
    echo '</pre>';
}

```


3. php获取客户端真实ip(即便他使用代理服务器)

```php

function getRealIpAddr() { 
    if (!empty($_SERVER['HTTP_CLIENT_IP'])) { 
        $ip=$_SERVER['HTTP_CLIENT_IP']; 
    } elseif (!empty($_SERVER['HTTP_X_FORWARDED_FOR'])) { 
        $ip=$_SERVER['HTTP_X_FORWARDED_FOR']; 
    } else { 
        $ip=$_SERVER['REMOTE_ADDR']; 
    } 
     
    return $ip; 
}


```


4. 文件下载

```php

function force_download($file)
{
    if ((isset($file)) && (file_exists($file))) {
        //这是一个文件流格式的文件
        Header("Content-type: application/octet-stream");
        //请求范围的度量单位--字节
        Header("Accept-Ranges: bytes");
        //Content-Length是指定包含于请求或响应中数据的字节长度
        Header("Accept-Length: " . filesize($file));
        //用来告诉浏览器，文件是可以当做附件被下载，下载后的文件名称为$file_name该变量的值。
        Header("Content-Disposition: attachment; filename=".date('Y-m-d-H:i:s',time()) . $file);
        readfile("$file");
    } else {
        echo "No file selected";
    }
}

// a链接下载文件
<a href="http://study.cn/doload.php">下载zip22</a>


```


5. 随机生成不重复的字符串

```php
function round_str($num){
    /**
     * 随机生成字符串 不重复的
     */

    // 合并数组 array_merge
    $arr = array_merge(range('A','Z'),range(0,9),range('a','z'),range('a','Z'));
    
    $rand_arr = [];
    for($i =0; $i < $num; $i++){
        // array_rand 返回数组中的一个随机键名
        $rand = $arr[array_rand($arr)];
        if(!in_array($rand,$rand_arr)){
            $rand_arr[] = $rand;
        }else{
            $i--;
        }
    }

    $rand_str = join('',$rand_arr);
    return $rand_str;

}

echo round_str(60);
```


6. 发送请求函数

```php
// 初始化 curl库
    public function initCurl($url,$data)
    {
        $options = array(
            'http' => array(
                'method'  => 'POST',
                'header'  => 'Content-type:text/plain',
                'content' => $data
                )
            );
        $context = stream_context_create($options);
        $resopse = file_get_contents($url,false,$context);

        if($resopse === false) return false;

        return json_decode($resopse);
    }

```


7. 




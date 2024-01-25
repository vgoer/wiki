---
title: 07.File文件
description: File文件
published: 1
date: 2023-06-09T10:15:57.605Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:29:48.941Z
---

<center>文件操作</center>

[toc]

## 文件操作

> 对操作文件



### 1.fopen()

>  fopen() 打开文件  是一个资源

```php
$res = fopen('one.txt','a+'); //当前目录下的one.txt a+可读可写

var_dump($res); //resource(3, stream)  是一个资源
```

```tex
"r" （只读方式打开，将文件指针指向文件头）
"r+" （读写方式打开，将文件指针指向文件头）
"w" （写入方式打开，清除文件内容，如果文件不存在则尝试创建之）
"w+" （读写方式打开，清除文件内容，如果文件不存在则尝试创建之）
"a" （写入方式打开，将文件指针指向文件末尾进行写入，如果文件不存在则尝试创建之）
"a+" （读写方式打开，通过将文件指针指向文件末尾进行写入来保存文件内容）
"x" （创建一个新的文件并以写入方式打开，如果文件已存在则返回 FALSE 和一个错误）
"x+" （创建一个新的文件并以读写方式打开，如果文件已存在则返回 FALSE 和一个错误）"r" （只读方式打开，将文件指针指向文件头）
```



### 2.fclose()

> 关闭文件，fclose很少使用,PHP有垃圾回收机制

```php
// 执行完一个页面后自动回收页面资源
fclose($res);
```



### 3.文件读取

#### a. fread()

> 从资源内读取数据 , 读取不重复

```php
//a.txt 内： aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
$res = fopen('a.txt',"a+");
$str1 = fread($res,10); //读取10个字符
$str2 = fread($res,100); //接着后面读取，到最后没有返回false
```



==拓展==:

> utf-8编码：*汉字占3个字符 空格占两个字符*



#### b.fgets()

> 每次读取文件的一行文本   *读取不重复*

```php
$str3 = fgets($res); //读取一行  最后false
//读取所有内容
while($a=fgets($res)){
    $a.=$a."<br>";
}
```



#### c.file_get_contents()

> 不需要使用资源,直接从文件内读取所有内容

```php
file_get_contents('a.txt'); 
```

```php
//读取图片要使用图片声明
header('Content-Type:jpge');
echo file_get_contents('one.jpg');
```



#### d. file()

> 函数将文件读取到**数组**中,各元素由**换行符分隔**。

```php
// 一行一行的读取 放在数组内
a.txt 
    aaa
    bbb
    ccc  ddd
    eee
$arr = file('文件路径'); //文件路径
print_r($arr);// ['aaa','bbb','ccc ddd','eee'];
```



### 4. filesize()

> 获取文件大小

```php
echo filesize('a.txt'); //几B
// 读取的文件和脚本文件  也要是utf-8编码

//写一个计算文件大小的函数 pow()
```

==拓展==:

> B-字节   Bit-比特位
>
> KB-千字节 MB GB TB

```text
1 Byte = 8 bits;  // 1字节=8比特(位)
1 KB = 1024 Bytes; //依次内推  1B(字节) 
```



### 5.文件写入

> 写入到文件内

#### a.file_put_contents()

> 直接将字符串覆盖存入文件内,新内容**覆盖**旧内容

```php
file_put_contents('a.txt','aadfasdsaf');  //第二个参是需要写入的内容
```



#### b.fwrite()  / fputs(安全)

> 不会覆盖文件内容  使用文件资源
>
> a+指针在文件最后,fwrite存入的内容会接着文件后面存入

```php
$res = fopen('b.txt','a+'); //a+指针在文件最后 文件不存在会创建

fwrite($res,'adafdsf'); // 第二个参是需要写入的内容
```



### 6.文件复制

> 复制文件 copy()

```php
copy('a.txt','b.txt');
```



### 7.重命名和剪切

> 重命名 rename()

```php
rename('a.txt','aa.txt');
rename('a.txt','aaa/bbb/a.txt');
```



### 8.删除文件

> 删除文件 unlink()

```php
unlink('aaa/bbb/a.txt');
```



### 9.文件时间

> 一定要设置时区   

```php
date_default_timezone_set('PRC'); //设置时区

// 文件创建的时间，返回时时间戳，所有格式化
filectime() //参数文件路径
echo date('Y-m-d H:i:s',filection('a.txt'));

filemtime() //文件最后内容修改的时间
echo date('Y-m-d H:i:s',filemtime('a.txt')); 

fileatime() //文件内容 权限等修改后的时间
echo date('Y-m-d H:i:s',fileatime('a.txt'));
```



### 10.判断文件

1. ==重要==file_exists() 

   > 判断文件是否存在，返回布尔值

   ```php
   var_dump(file_exists('a.txt')); //true
   ```

2. is_readable()     is_writeable() 可写

   > 判断文件是否可读

   ```php
   var_dump(is_readable('a.txt')); //true
   ```

3. is_dir()

   > 判断文件是否是文件夹

   ```php
   var_dump(is_dir('a.txt'));//false
   ```

4. is_file()

   > 判断是否是文件

   ```php
   var_dump(is_file('a.txt')); //true
   ```

5. feof()

   > 测是否已到达文件末尾 (eof)。

   ```php
   //读取全部内容
   $file = fopen("test.txt", "r");
   while(! feof($file))
     {
     echo fgets($file). "<br />";
     }
   
   fclose($file);
   ```

6. filetype()

> 返回文件类型

```php
echo filetype('a.txt');  //file
```



7. 指针(鼠标)位置  fseek()   frwind()

```php

$res3 = fopen('a.txt','r'); //只读，指针指向开头

//fwrite('$res3','bbb');
fseek($res3,20);  //改变指针 在20b的位置

frwind($res3);  //文件指针的位置倒回文件的开头。

$str2 = fread($res3,filesize('a.txt'));

```



8. tmpfile()   临时文件

```php
tmpfile() 函数以读写（w+）模式建立一个具有唯一文件名的临时文件。
文件会在关闭后（用 fclose()）自动被删除，或当脚本结束后。
$tmp = tmpfile();
```



### 11.文件名

> 文件路径：

```php
// 1. 当前文件路径
echo __FILE__;
echo __DIR__;

// 右斜杠在服务器linux无效
$path = str_replace('\\','/',__FILE__);  //字符串替换

// 2. dirname() 只返回路径的文件夹部分
echo dirname('../def/a.txt'); //文件夹部分

// 3. basename()  两个参数
echo basename($path);  //  返回文件名
echo basename($path,'后缀名') // 只返回文件名称不包含后缀

// 4.pathinfo()  将完整路径解析后存储到数组内
打印数组处来看   pathinfo($p);
echo pathinfo($p)['extension'];  //后缀名
echo pathinfo($p,PATHINFO_EXTESION); //后缀名
echo pathinfo($P,PATHINFO_BASENAME); // 获取文件完整名称
echo pathinfo($P,PATHINFO_DIRNAME); // 获取文件路径的目录部分
```



### 文件包含

> 引入文件

```
1. include  
包含并运行指定文件,如果没有给出目录（只有文件名）时则按照 include_path指定的目录寻找。如果在 include_path下没找到该文件则 include 最后才在调用脚本文件所在的目录和当前工作目录下寻找。如果最后仍未找到文件则 include 结构会发出一条警告这一点和require不同，后者会发出一个致命错误

2. include_once
在脚本执行期间包含并运行指定文件，唯一区别是如果该文件中已经被包含过，则不会再次包含。如同此语句名字暗示的那样，只会包含一次。

3. require
　require 和 include几乎完全一样，除了处理失败的方式不同之外。require 在出错时产生E_COMPILE_ERROR 级别的错误。换句话说将导致脚本中止而 include只产生警告（E_WARNING），脚本会继续运行。

4. require_once
require_once 语句和 require 语句完全相同，唯一区别是 PHP 会检查该文件是否已经被包含过，如果是则不会再次包含。

```


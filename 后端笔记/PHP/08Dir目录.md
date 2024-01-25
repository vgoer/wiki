---
title: 08.Dir目录
description: Dir目录
published: 1
date: 2023-06-09T10:15:59.782Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:30:33.027Z
---

<center>目录/文件夹</center>

[toc]

## 目录操作

> 操作我们的文件夹

### 1.opendir()

> 打开目录， 是一个资源

```php
$res = opendir('aaa');  //打开aaa文件夹

var_dump($res);//resource(4, stream)  //资源
```



### 2.读取目录

#### a.readdir()

> 读取资源内的文件夹     读取完 返回false

```php
// .  .. 两个隐藏目录，所有文件下都有 
echo readdir($res)."<br>";  // .
echo readdir($res)."<br>";  // ..
echo readdir($res)."<br>";  // a.txt

// 1次读取全部
while($a = readdir($res)){
    echo $a."<br>";
}
```



#### b.scandir()

> 不需要读取资源，直接读取到数组内 **常用**	

```php
$arr = scandir('aaa'); //读取aaa目录下所有的文件
uset($arr[0]);
uset($arr[1]);
/*Array
(
    [2] => a.txt
    [3] => b.txt
    [4] => one.jpg
)*/
// 删除隐藏的文件，但是我们索引这些都没有变 从2开始的数组
```

==数组拓展==：

```php
//  array_values重置数组索引

/*Array
(
    [0] => a.txt
    [1] => b.txt
    [2] => one.jpg
)*/
//这样就可以了

//  array_keys 提取键值变成索引数组
$arr2 = [
    'aa'=>1,
    'bb'=>2,
    'cc'=>3,
]; 
Array
(
    [0] => aa
    [1] => bb
    [2] => cc
)
//提取键值
```



### 4. 删除目录

> 只能删除空白目录，如果要删除任意目录，就必须去目录下删除里面的文件
>
> > 只删除文件夹内的空文件夹

```php
rmdir('bbb');   //只能删除空白文件夹

//删除任意文件
function rm_rmdir($path){
    /* rm_rmdir 删除任意文件
        $path 想要删除的目录 */
    $arr = scandir($path);
    foreach($arr as $value){
        if($value == '.' || $value == '..'){
            continue;
        }

        // 拼接下一个目录
        $file = $path."/".$value;
        // echo $file;
        if(is_file($file)){
            unlink($file);
        }else{
            //递归函数，如果是非空目录，有执行函数
            rm_rmdir($file);
        }
    }
    rmdir($path);

}

rm_rmdir('aaa');
// 完成只删除空白目录的函数
```



### 5.创建文件夹

> mkdir() 创建文件夹

```php
mkdir('aaaa');  //当前目录中 创建 文件夹
mkdir('a/b/c/d/e', 0777,true); 
// 多级目录   0777 管理权限
```



> getcwd() 返回当前目录

```php
echo getcwd()
```






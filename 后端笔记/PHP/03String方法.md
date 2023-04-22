---
title: 03.String 方法
description: String 方法
published: 1
date: 2023-04-22T17:24:59.760Z
tags: php
editor: markdown
dateCreated: 2023-04-22T17:24:57.088Z
---

<center>字符串方法</center>

[toc]

## String 方法

> echo & print

```php
// 打印字符串
echo "hello";
print("hello");
```



> printf()

```php
// 格式化输出字符串
/** 
 * 1. %b  二进制数   %o 八进制   %x 十六进制
 * 2. %c,(1010)  依照 ASCII 值的字
 * 3. %d  整型
 * 4. %f  浮点数  %.3f -> 保留3位小数
 * 5. %s  字符串
 */

$str = "123 abc";
printf("整数：%d",$str);
printf("浮点数：%.3f",$str);
```



> var_dump() 打印变量的数据类型和值

```php
$arr = range('a','z');
var_dump($arr);
```



> `html_entity_decode() `函数把 HTML 实体转换为字符。

```php
$str = "i am &nbsp; gogo&gt;";

echo html_entity_decode($str); // i am  ; gogo>;
```

> 上面的下面的两个函数的有第二个参数：`规定如何解码单引号和双引号。 ENT_COMPAT - 默认。仅解码双引号。 ENT_QUOTES - 解码双引号和单引号。 ENT_NOQUOTES - 不解码任何引号。`

> htmlspecialchars() 将标签解析为html实体输出

```php
$str = "John & 'Adams'";

echo htmlspecialchars($str, ENT_QUOTES); // John &amp; &#039;Adams&#039
```



> join()   把数组元素组合为一个字符串
>
> >  就是 implode()

```php
$arr = range('a','e');
print_r join('',$arr); // str = abcde
```



> ltrim(),rtrim()和trim()删除空格或其他预定义字符

```php
// ltrim()  左侧删除
// rtrim()   右侧删除
// trim()    左右都删除
$str = " sfdasf bac & ";
echo trim($str); //sfdasf bac &
echo trim($str,'aa'); //sfdasf bac &
```



> ==重要==: md5() 计算字符串的 MD5 散列。 md5+加盐 保存密码

```php
// md5() 函数使用 RSA 数据安全，包括 MD5 报文摘译算法。
// 加盐函数 -- 26个英文随机输出四个，拼接密码 md加密

$str = '123455';
echo md5($str);  
//任何字符串转换为固定的32位字符串
```

```php
function salting()
{
    /**
         * 加盐函数， 随机生产4个英文字母
         */
    $arr = range('a','z');
    print_pre($arr);
    $salting_arr = [];
    for($i=0;$i<4;$i++){
        $rand = $arr[array_rand($arr)];
        if(!in_array($rand,$salting_arr)){
            $salting_arr[] = $rand;
        }else{
            $i--;
        }
    }
    // return $salting_arr;
    $rand_str = join('',$salting_arr);
    return $rand_str;
}
```



> nl2br() nl2br() 函数在字符串中的每个新行 (\n) 之前插入 HTML 换行符 (\<br />)。

```php
$str = "one \n dsafsd
dafsdf
fdasfd\n";

echo nl2br($str); // 只要有\n和换行的地方都会br
```



> ==重要==sha1()  函数计算字符串的 SHA-1 散列。

```php
// 也是加密

$password = "12345";
echo sha1($password);
```



> sha1_file()  计算文件的 SHA-1 散列。

> md5_file()  计算文件的 MD5 散列。

```php
// 都是 验证文件的完整性 进行验证

$filename = "a.txt";    //获取到文件
echo md5_file($filename);
file_put_contents() // 存入覆盖文件
file_get_contents() // 读取文件全部内容
```



> **str_replace()**  str_ireplace() 都是 使用一个**字符串替换**字符串中的另一些字符。

| 参数    | 描述                               |
| :------ | :--------------------------------- |
| find    | 必需。规定要查找的值。             |
| replace | 必需。规定替换 find 中的值的值。   |
| string  | 必需。规定被搜索的字符串。         |
| count   | 可选。一个变量，对替换数进行计数。 |

```php
// 注意参数的位置
$str = "hello world aaabbb";
echo str_replace('a','b',$str); // hello world bbbbbb替换全部
echo str_replace('a','b',$srt,4); //参数

//str_replace区分大小写
//str_ireplace不区分大小写
```



> strpos()    stripos()       strripos() 找到字符串**位置**

```php
// strpos()  区分大小写， 找到另一个字符第一次出现的位置
// stripos()  不区分大小写， 找到另一个字符第一次出现的位置
// strripos() 不区分大小写， 找到另一个字符最后一次出现的位置

echo strripos($str,'ll'); // 14 索引
```



> strstr() / strchr()一样        stristr()/strichr    找到字符串的**其他位置**

 ```php
 //strstr() 函数查找字符串在另一个字符串中第一次出现的位置。 则返回字符串的其余部分（从匹配点） 不区分大小写 包含自己
 // stristr() 区分大小写 
 
 $str = "hello world";
 echo strstr($str,'l');  // 区分大小写  llo world
 ```



> str_repeat()  函数把**字符串重复指定的次数**。

```php
// 重复次数
echo str_repeat($str,'l'); //3
```



> str_shuffle()   函数**随机地打乱字符串中的所有字符。**

```php
$str = 'abcdef
echo str_shuffle($str);  //随机
```



> ==重要==str_split()   字符串分割到数组中。

| 参数   | 描述                                     |
| :----- | :--------------------------------------- |
| string | 必需。规定要分割的字符串。               |
| length | 可选。规定每个数组元素的长度。默认是 1。 |

```php
// 和explode() 
$str = 'hello world cc';

$arr = explode(' ',$str);  // ['hello','world','cc']

//str_split() 用法不同
$arr = str_split($str,3); //每个数组的长度 3 
```



> strip_tages()  数剥去 HTML、XML 以及 PHP 的标签。

| 参数   | 描述                                       |
| :----- | :----------------------------------------- |
| string | 必需。规定要检查的字符串。                 |
| allow  | 可选。规定允许的标签。这些标签不会被删除。 |

```php
echo strip_tags("Hello <b>world!</b>");  // b标签删除
// 取消b标签，删除i标签
echo strip_tags("Hello <b><i>world!</i></b>","<b>");
```



> ==重要==strlen()  返回字符串的长度。

```php
echo strlen("hello"); //5
```



> strrev() 反转字符串。

```php
echo strrev("hello");  //olleh
```



> ==重要==strtolower()     strtoupper()**字符串转换为小大写**。

```php
// strtolower()  小写
echo strtolower("HELLO")；
// strtoupper()  echo strtolower(“HELLO”)大写
```



> ==重要== substr()  **字符串截取。**

| 参数   | 描述                                                         |
| :----- | :----------------------------------------------------------- |
| string | 必需。规定要返回其中一部分的字符串。                         |
| start  | 必需。规定在字符串的何处开始。 正数 - 在字符串的指定位置开始 负数 - 在从字符串结尾的指定位置开始 0 - 在字符串中的第一个字符处开始 |
| length | 可选。规定要返回的字符串长度。默认是直到字符串的结尾。 正数 - 从 start 参数所在的位置返回 负数 - 从字符串末端返回 |

```php
// 很强大
echo substr("aaaaaabbbb",5,4);
```



> ucfirst() 字符串中的首字符转换为大写。

> ucwords()  字符串中每个单词的首字符转换为大写。

```php
echo ucfirst("hello world"); //Hello world
echo ucwords("hello world");  //Hello World
```

> nl2br()  字符串内的文本换行符变成html换行符\<br>

```php 
$str = "aa
bb
cc";
echo nl2br($str); 
```




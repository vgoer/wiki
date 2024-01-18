<center>SPL标准库</center>





[toc]









## SPL

> PHP 标准库（SPL）是用于解决标准问题（standard problems）的一组接口与类的集合。[php](https://www.php.net/manual/zh/book.spl.php)
>
> 安装： php的核心。不需要安装







### 1. SplFileInfo 

> SplFileInfo 类为单个文件的信息提供了一个高级的面向对象的接口。

```php
$fileinfo = new SplFileInfo("a.text");

// 真实的路径
echo $realpath = $fileinfo->getRealPath(); echo "<br>";

// 文件类型
echo $filetype = $fileinfo->getType(); echo "<br>";

// 文件大小
echo $fileSize = $fileinfo->getSize(); echo "<br>";

// 获取扩展名
echo $fileExten = $fileinfo->getExtension(); echo "<br>";

var_dump($fileinfo);
```









### 2. SplStack 

> SplStack 类的主要功能是通过将迭代模式设置为 **`SplDoublyLinkedList::IT_MODE_LIFO`** 来提供使用双向链表实现的栈。

```php
$q = new SplStack();

# 添加元素
$q[] = 1;
$q[] = 2;

$q->push(3);

var_dump($q);
```












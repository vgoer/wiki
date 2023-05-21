<center>Havefun</center>



[toc]

## Havefun

> [havefun](https://buuoj.cn/challenges#[%E6%9E%81%E5%AE%A2%E5%A4%A7%E6%8C%91%E6%88%98%202019]Havefun)  其实就是查看源码



## 1. payload

> 查看源码发现

```php
        <!--
$cat=$_GET['cat'];
echo $cat;
if($cat=='dog'){
    echo 'Syc{cat_cat_cat_cat}';
}
-->
```

> 发送一个get请求

```shell
url?cat=dog

# 获取到了
flag{474219e5-4cdd-444b-8e5b-7acd59f0d6c5}
```






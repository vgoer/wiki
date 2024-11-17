<center>Brute Force</center>





[toc]









## Brute Force

> 暴力破解







### 1. low

> 低级

> 看代码 [url](http://192.168.0.111:8000/vulnerabilities/view_source.php?id=brute&security=low)

```shell
# docker
docker exec -it dvwa bash

# 进入目录 
cd /var/www/html

# 查看源码
```

> sql注入问题：

```shell
# sql 语句
SELECT * FROM `users` WHERE user = '$user' AND password = '$pass';

# admin' or '1'='1
SELECT * FROM `users` WHERE user = admin' or '1'='1 AND password = '$pass'
```





### 2. medium

> 中级
>
> 源码：[url](http://192.168.0.111:8000/vulnerabilities/view_source.php?id=brute&security=medium)

mysqli_real_escape_string [菜鸟](https://www.runoob.com/php/func-mysqli-real-escape-string.html)

多了这个过滤方法： 

```shell
转义特殊字符： 编码的字符是 NUL（ASCII 0）、\n、\r、\、'、" 和 Control-Z。
```

> 开始暴力破解：brup抓包。

```shell
1. proxy 抓包

2. 发送到intruder

3. 对密码进行爆破，看length。 不一样内个可能就是密码
```






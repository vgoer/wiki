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


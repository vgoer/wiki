<center>Preignition</center>





[toc]









## Preignition

> Preignition









### 1. task

1. Directory Brute-forcing is a technique used to check a lot of paths on a web server to find hidden pages. Which is another name for this? (i) Local File Inclusion, (ii) dir busting, (iii) hash cracking.

```shell
dir busting
```

2. What switch do we use for nmap's scan to specify that we want to perform version detection

```shell
-sV
```

3. What does Nmap report is the service identified as running on port 80/tcp?

```shell
http
```

4. What server name and version of service is running on port 80/tcp?

```shell
nginx 1.14.2
```

5. What switch do we use to specify to Gobuster we want to perform dir busting specifically?

```shell
gobuster --help

dir
```

6. When using gobuster to dir bust, what switch do we add to make sure it finds PHP pages?

```shell
-x php
```

7. What page is found during our dir busting activities?

```shell
admin.php
```

8. What is the HTTP status code reported by Gobuster for the discovered page?

```shell
200
```







### 3. flag

> 获取flag

```shell
http://10.129.52.71/admin.php

# 尝试弱密码，或者之间爆破
admin/admin
```


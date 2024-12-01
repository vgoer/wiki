<center>Lgnition</center>





[toc]







## Lgnition

> Lgnition







### 1. task

1. Which service version is found to be running on port 80?

```shell
nginx 1.14.2
```

2. What is the 3-digit HTTP status code returned when you visit http://{machine IP}/?

```shell
302
```

3. What is the virtual host name the webpage expects to be accessed by?

```shell
ignition.htb
```

4. What is the full path to the file on a Linux computer that holds a local list of domain name to IP address pairs?

```shell
/etc/hosts
```

5. Use a tool to brute force directories on the webserver. What is the full URL to the Magento login page?

```shell
http://ignition.htb/admin
```

6. Look up the password requirements for Magento and also try searching for the most common passwords of 2023. Which password provides access to the admin account?

```shell
qwerty123
```







### 3. flag

> 获取flag

```shell
nmap -sC -sV IP
```

![image-20241201102755800](./assets/image-20241201102755800.png)

```shell
# 加入hosts文件
vim /etc/hosts

IP   ignition.htb
```

```shell
# 目录扫描
gobuster dir -u  http://ignition.htb/ -w directory-list-2.3-small.txt 


http://ignition.htb/admin

搜索Magento相关信息可知，默认账户admin，密码为7个及以上字符，但是反暴力破解，尝试手工测试常见密码。测试结果为admin：
admin/qwerty123
```


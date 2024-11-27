<center>Responder</center>



[toc]







## Responder

> Responder





### 1. task

1. When visiting the web service using the IP address, what is the domain that we are being redirected to?

```shell
unika.htb
```

2. Which scripting language is being used on the server to generate webpages?

![image-20241127160422068](./assets/image-20241127160422068-1732694665608-1.png)

```shell
php

# 跳转，修改本机hosts文件
sudo vim /etc/hosts

服务器ip 
```

3. What is the name of the URL parameter which is used to load different language versions of the webpage?

```shell
http://unika.htb/index.php?page=french.html
page
```

4. Which of the following values for the `page` parameter would be an example of exploiting a Local File Include (LFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe"

```shell
http://unika.htb/index.php?page=../../../../../../../../windows/system32/drivers/etc/hosts

../../../../../../../../windows/system32/drivers/etc/hosts
```

5. Which of the following values for the `page` parameter would be an example of exploiting a Remote File Include (RFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe"

```shell
//10.10.14.6/somefile
```

6. What does NTLM stand for?

```shell
New Technology LAN Manager
```

7. Which flag do we use in the Responder utility to specify the network interface?

```shell
-l
```

8. There are several tools that take a NetNTLMv2 challenge/response and try millions of passwords to see if any of them generate the same response. One such tool is often referred to as `john`, but the full name is what?.

```shell
john the ripper
```

9. What is the password for the administrator user?

```shell
badminton
```

10. We'll use a Windows service (i.e. running on the box) to remotely access the Responder machine using the password we recovered. What port TCP does it listen on?

```shell
5985
```






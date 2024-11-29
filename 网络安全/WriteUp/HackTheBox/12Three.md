<center>Three</center>





[toc]





## Three

> Three







### 1 .task

1. How many TCP ports are open?

```shell
nmap -v -p- --min-rate 5000 -sV -sC 10.129.191.68

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 17:8b:d4:25:45:2a:20:b8:79:f8:e2:58:d7:8e:79:f4 (RSA)
|   256 e6:0f:1a:f6:32:8a:40:ef:2d:a7:3b:22:d1:c7:14:fa (ECDSA)
|_  256 2d:e1:87:41:75:f3:91:54:41:16:b7:2b:80:c6:8f:05 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: The Toppers
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

2
```

2. What is the domain of the email address provided in the "Contact" section of the website?

```shell
thetoppers.htb
```

3. In the absence of a DNS server, which Linux file can we use to resolve hostnames to IP addresses in order to be able to access the websites that point to those hostnames?

```shell
/etc/hosts
```

4. Which sub-domain is discovered during further enumeration?

```shell
# 对子域名进行爆破
gobuster vhost -u http://thetoppers.htb -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt --append-domain

Found: s3.thetoppers.htb Status: 404 [Size: 21]
Found: gc._msdcs.thetoppers.htb Status: 400 [Size: 306]

s3.thetoppers.htb
```

5. Which service is running on the discovered sub-domain?

```shell
# 将 s3.thetoppers.htb 添加到 hosts文件
10.129.191.68 thetoppers.htb s3.thetoppers.htb

Amazon S3
```

> s3: Amazon Simple Storage Service （Amazon S3） 是由 Amazon Web Services （AWS） 提供的公共云服务。Amazon S3 通过简单的 Web 服务接口提供对象存储。它被广泛用于存储照片，视频，文本文件，文档，PDF文件，并存储大量数据的备份。

6. Which command line utility can be used to interact with the service running on the discovered sub-domain?

```shell
sudo apt install awscli
awscli
```

7. Which command is used to set up the AWS CLI installation?

```shell
aws configure
```

8. What is the command used by the above utility to list all of the S3 buckets?

```shell
# 帮助文档
tldr aws s3

aws s3 ls
```

9. This server is configured to run files written in what web scripting language?

```shell
aws s3 --endpoint=http://s3.thetoppers.htb ls s3://thetoppers.htb
# you php 文件
php 
```







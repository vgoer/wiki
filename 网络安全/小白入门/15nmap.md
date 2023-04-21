---
title: 15.nmap
description: nmap端口扫描器
published: 1
date: 2023-04-21T05:03:09.381Z
tags: web safe, nmap
editor: markdown
dateCreated: 2023-04-19T17:35:33.986Z
---

<center>Nmap</center>



[toc]



## nmap

> 强大的主机发现和端口扫描工具

官网：[nmap](https://nmap.org)

> 看`官网的教程`，和学会看`参数帮助文档`



1. ip/域名

```shell
nmap + ip/域名
nmap + ip/24 扫描一个网段
```



2. `-iL` 扫描一个IP文件

```shell
nmap -iL ip.txt   //IP文件
```



3. `iR`随机目标

```shell
nmap -iR 3 -v //随机3个目标  -v详细点信息
nmap -iR 1 -vv // -vv更详细些
```



4. `--exclude`排除文件内不扫描的ip/域名
5. `--excludefile`排除不想扫描的IP文件

```
nmap -iL ip.txt --exclude baidu.com //排除baidu的域名ip
nmap -iL ip.txt --dexcludefile ip2.txt //排除ip文件
```



### 主机发现

> 看主机是否在线

1. -sp *用*

> 仅仅是ping扫描，可能会有SYN扫描

```
nmap -sp ip
```



2. -p0(无ping)

> 跳过ping，直接高强度扫描主机

```
nmap -p0 ip
```



3. -PS [port]\(TCP SYN Ping)

> tcp的SYN字段去ping

```
nmap -PS 80,443,23,22
```



4. -PA [port]\(TCP ACK Ping)

> tcp 的 ack字段ping



5. -PU[port]

> upd 的 ping



6. -PR(ARP ping)

> arp的ping



### 端口扫描

> 指定端口扫描

1. -p

```
nmap -p 80,443,22 +ip  //指定端口
T:80  U:100  //tcp的80 和udp的100
nmap -p T:80,U:442 
```

2. -F              -r

> 快扫          不按顺序来扫

```
nmap -Fp 80,31,313,10   //快速扫描
```



### 端口扫描技术

> 扫描端口

1. -sS (TCP SYN)扫描  **常用**

> SYN半开扫描，一次握手就确认了



> 最受欢迎的扫描，执行快，不张扬，任意平台

```
nmap -sS -p 80,443 +ip   //半开放扫描
```



2. -sT(TCP connect())扫描

> SYN不能用时可以用，默认建立TCP扫描，容易被主机发现

```
nmap -sT -p 80 +ip
```



3. -sU(UPD扫描)

```
nmap -sU -p 20 +ip
```



4. -sv 版本

```shell
nmap -sV ip
```



5. -T 时间

```shell
namp -T 1/5 ip
```





### 脚本扫描

> 爆破某一端口 [github](https://github.com/nmap/nmap/tree/master/scripts)

```shell
nmap ip --script=脚本
```



> -D 诱饵扫描 让主机无法分辨我们真实ip

```shell
nmap -D 192.168.1.1,***.2,ME -p 80 -sS ip
# 192.168.1.1,***.2,ME 三个ip去扫
```



### 端口定义

> open 和 closed  和 不确定是否开




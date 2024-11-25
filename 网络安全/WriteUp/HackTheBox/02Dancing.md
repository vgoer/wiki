<center>Dancing</center>





[toc]









## Dancing

> Dancing 靶机







### 1. task

1. What does the 3-letter acronym SMB stand for?

```shell
Server Message Block
```

2. What port does SMB use to operate at?

```shell
445
```

3. What is the service name for port 445 that came up in our Nmap scan?

```shell
microsoft-ds
```

4. What is the 'flag' or 'switch' that we can use with the smbclient utility to 'list' the available shares on Dancing?

```shell
-L
```

5. How many shares are there on Dancing?

```shell
4
```

6. What is the name of the share we are able to access in the end with a blank password?

```shell
# 不使用密码就可以登陆的用户名。
WorkShares
```

7. What is the command we can use within the SMB shell to download the files we find?

```shell
# help
get
```





### 2. SMB

> SMB (Server Message Block) 是一个网络文件共享协议。[blog](https://blog.51cto.com/u_13858192/2155437)

```shell
文件和打印机共享
网络文件系统访问
网络间通信
远程服务调用

默认端口：
TCP 445 (直接通过 TCP/IP)
TCP 139 (通过 NetBIOS)
```

> smb服务端和客户端搭建。

```shell
1. Linux搭建samba

a. 安装
# Debian/Ubuntu
sudo apt update
sudo apt install samba

# CentOS/RHEL
sudo dnf install samba

b. 配置
# 备份原配置
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak

# 编辑配置文件
sudo nano /etc/samba/smb.conf
```

```shell
[global]
   workgroup = WORKGROUP
   server string = Samba Server
   security = user
   map to guest = bad user
   dns proxy = no

[Share]
   path = /samba/share
   browseable = yes
   writable = yes
   guest ok = yes
   read only = no
   create mask = 0777
   directory mask = 0777
```

```shell
# 创建共享目录
sudo mkdir -p /samba/share
sudo chmod 777 /samba/share

# 创建 Samba 用户
sudo smbpasswd -a username

# 重启服务
sudo systemctl restart smbd
sudo systemctl enable smbd


2. windows搭建 SMB服务器

启用 SMB 服务
a. 打开控制面板
b. 程序和功能 -> 启用或关闭 Windows 功能
c. 勾选 "SMB 1.0/CIFS File Sharing Support

# 共享文件
# PowerShell 创建共享
New-SmbShare -Name "Share" -Path "C:\Share" -FullAccess "Everyone"

# 或使用图形界面
# 右键文件夹 -> 属性 -> 共享 -> 高级共享
```

> 客户端

```shell
1. linux客户端
# 安装客户端工具
sudo apt install smbclient cifs-utils

# 列出共享
smbclient -L //server_ip -U username

# 挂载共享
sudo mkdir /mnt/share
sudo mount -t cifs //server_ip/Share /mnt/share -o username=username,password=password

# 永久挂载（编辑 /etc/fstab）
//server_ip/Share /mnt/share cifs username=username,password=password 0 0

2. window
# 命令行映射网络驱动器
net use Z: \\server_ip\Share password /user:username

# 或使用图形界面
# 此电脑 -> 映射网络驱动器
```





### 3. flag

> 获取flag

```shell
# namp 获取端口
nmap 10.129.169.185 -A 

# 
mbclient -L 10.129.169.185

# 进入smb  \\\\ip\\用户
smbclient \\\\10.129.169.185\\WorkShares

# 查看命令
help

# 下载flag
get James.P\flag.txt

# 查看 下载到本地了。
cat flag.txt 
5f61c10dffbc77a704d76016a22f1664 
```


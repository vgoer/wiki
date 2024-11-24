<center>Fawn</center>







[toc]







## Fawn

> fawn







### 1. task

1. What does the 3-letter acronym FTP stand for?

```shell
File Transfer Protocol
```

2. Which port does the FTP service listen on usually?

```shell
21
```

3. FTP sends data in the clear, without any encryption. What acronym is used for a later protocol designed to provide similar functionality to FTP but securely, as an extension of the SSH protocol?

```shell
sftp
```

4. What is the command we can use to send an ICMP echo request to test our connection to the target?

```shell
ping 
```

5. From your scans, what version is FTP running on the target?

```shell
nmap -p 21 10.129.13.222 -A

21/tcp open  ftp     vsftpd 3.0.3
```

6. From your scans, what OS type is running on the target?

```shell
Service Info: OS: Unix
```

7. What is the command we need to run in order to display the 'ftp' client help menu?

```shell
ftp -?  
```

8. What is username that is used over FTP when you want to log in without having an account?

```shell
# 用户通常使用 ftp 或 anonymous 的用户名登录
anonymous
```

9. What is the response code we get for the FTP message 'Login successful'?

```shell
230 FTP Response code
```

10. There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system.

```shell
ls
```

11. What is the command used to download the file we found on the FTP server?

```shell
get
```



### 2. ftp

> [ftp vs sftp](https://cloud.tencent.com/developer/article/2279327)文件服务器

> vsftpd服务端和客户端搭建。

```shell
# Ubuntu/Debian
sudo apt update
sudo apt install vsftpd

# CentOS/RHEL
sudo yum install vsftpd

# 编辑配置文件
sudo nano /etc/vsftpd.conf
```

```txt
# 允许本地用户登录
local_enable=YES

# 允许上传
write_enable=YES

# 本地用户根目录
local_root=/home/ftp

# 启用 ASCII 模式
ascii_upload_enable=YES
ascii_download_enable=YES

# 欢迎信息
ftpd_banner=Welcome to FTP Server

# 限制用户在其主目录
chroot_local_user=YES
```

```shell
# 启动服务
sudo systemctl start vsftpd

# 设置开机启动
sudo systemctl enable vsftpd

# 检查状态
sudo systemctl status vsftpd
```

```shell
# 客户端
# Ubuntu/Debian
sudo apt install ftp

# CentOS/RHEL
sudo yum install ftp

# 基本连接
ftp hostname_or_ip

# 指定端口连接
ftp -p hostname_or_ip port
```

> 文件操作命令

```shell
# 列出文件
ls
dir

# 切换目录
cd directory_name

# 显示当前目录
pwd

# 下载文件
get filename
mget *.txt    # 下载多个文件

# 上传文件
put filename
mput *.txt    # 上传多个文件

# 删除文件
delete filename
mdelete *.txt # 删除多个文件

# 创建目录
mkdir dirname

# 删除目录
rmdir dirname

# 显示帮助
help

# 显示本地目录
!ls
!dir

# 切换本地目录
lcd directory_name

# 退出
bye
quit
exit

# 创建用户
sudo useradd -m ftpuser
sudo passwd ftpuser

# 创建FTP目录
sudo mkdir -p /home/ftp
sudo chown ftpuser:ftpuser /home/ftp

# 设置目录权限
sudo chmod 755 /home/ftp
```





### 3. 获取flag

```shell
# 登陆 ftp
ftp 10.129.13.222 

Connected to 10.129.13.222.
220 (vsFTPd 3.0.3)
Name (10.129.13.222:goer): anonymous
331 Please specify the password.
Password: 
230 Login successful.


# 用户名和密码
anonymous/anonymous

# ls 查看目录
ls

# 下载文件
get flag.txt

# 推出
exit

# 查看
cat flag.txt
035db21c881520061c53e0536e44f815
```


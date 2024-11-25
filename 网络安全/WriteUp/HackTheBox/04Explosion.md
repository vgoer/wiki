<center>Explosion</center>







[toc]







## Explosion

> Explosion







### 1. task

1. What does the 3-letter acronym RDP stand for?

```shell
# 远程桌面协议
Remote Desktop Protocol
```

2. What is a 3-letter acronym that refers to interaction with the host through a command line interface?

```shell
command line interface
cli
```

3. What about graphical user interface interactions?

```shell
gui
```

4. What is the name of an old remote access tool that came without encryption by default and listens on TCP port 23?

```shell
telnet
```

5. What is the name of the service running on port 3389 TCP?

```shell
nmap -sV 10.129.172.157
# ms 
ms-wbt-server
```

6. What is the switch used to specify the target host's IP address when using xfreerdp?

```shell
# 安装 xfreerdp
sudo apt install freerdp2-x11

xfreerdp --help

/v:
```

7. What username successfully returns a desktop projection to us with a blank password?

```shell
Administrator
```







### 2. RDP协议

> RDP 是微软开发的专有协议。[blog](https://www.cnblogs.com/88223100/p/RDP-Protocol-Details.html)

```shell
远程桌面访问
远程应用程序访问
文件传输
音频重定向
打印机重定向
```

> 默认端口：`3389`



```shell
1. windowns 服务端配置

# PowerShell 启用 RDP
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0

# 设置 RDP 端口（可选）
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "PortNumber" -Value 3389


# 配置防火墙规则
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
```

或通过图形界面：

* 系统属性 -> 远程

* 选择"允许远程连接到此计算机"

* 选择用户

```shell
2. linux 服务端（XRDP）
# 安装 XRDP
sudo apt update
sudo apt install xrdp

# 启动服务
sudo systemctl enable xrdp
sudo systemctl start xrdp

# 配置文件 
cat /etc/xrdp/xrdp.ini

[Globals]
; xrdp.ini 基本配置
max_bpp=32
xserverbpp=24
port=3389
crypt_level=high
channel_code=1
```

```shell
3. Windows客户端

# 命令行启动远程桌面
mstsc.exe /v:server_ip:port
```

或使用图形界面：

* 开始菜单搜索"远程桌面连接"

* 输入服务器地址

* 输入用户名密码

```shell
4. linux 客户端

# 安装 Remmina
sudo apt install remmina remmina-plugin-rdp

# 或安装 FreeRDP
sudo apt install freerdp2-x11

# FreeRDP 命令行连接
xfreerdp /v:server_ip:port /u:username /p:password
```







### 3. flag

> 获取flag

```shell
# 登陆windows server
xfreerdp /v:10.129.172.157 /u:Administrator /p:
```


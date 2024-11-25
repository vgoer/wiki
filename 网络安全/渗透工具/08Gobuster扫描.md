<center>Gobuster扫描</center>



[toc]









## Gobuster扫描

> Gobuster 是一个用 Go 语言编写的目录/文件爆破工具，主要用于网站目录扫描和 DNS 子域名爆破。它的特点是速度快、功能强大且使用简单。
>
> [github](https://github.com/OJ/gobuster)





### 1. 主要功能

1. 目录扫描模式 (dir)

扫描网站目录和文件

支持自定义字典

可以指定文件扩展名

2. DNS 子域名扫描模式 (dns)

扫描子域名

支持通配符检测

3. 虚拟主机扫描模式 (vhost)

发现虚拟主机名

基于响应大小进行过滤





### 2. 安装

```shell
# 使用 apt 安装（Debian/Ubuntu）
sudo apt install gobuster

# 使用 go 安装
go install github.com/OJ/gobuster/v3@latest
```







### 3. 常用命令

```shell
1. 目录扫描

# 基本用法
gobuster dir -u http://example.com -w /usr/share/wordlists/dirb/common.txt

# 指定文件扩展名
gobuster dir -u http://example.com -w wordlist.txt -x php,html,txt

# 使用代理
gobuster dir -u http://example.com -w wordlist.txt -p http://127.0.0.1:8080

2. DNS 子域名扫描
# 基本用法
gobuster dns -d example.com -w subdomains.txt

# 显示找到的 IP
gobuster dns -d example.com -w subdomains.txt -i

# 虚拟主机扫描
gobuster vhost -u http://example.com -w vhost-wordlist.txt
```





### 4. 注意事项

使用前确保已获得授权，未经允许扫描他人系统是违法行为

注意控制扫描速度，避免对目标系统造成影响

建议在测试环境中先熟悉工具的使用


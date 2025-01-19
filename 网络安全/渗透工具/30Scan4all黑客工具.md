<center>Scan4all黑客工具</center>







[toc]









## Scan4all黑客工具

> 漏洞扫描；15000+PoC漏洞扫描；[ 23 ]种应用弱口令爆破；7000+Web指纹；146种协议90000+规则端口扫描；Fuzz、HW打点、BugBounty神器... [github](https://github.com/GhostTroops/scan4all)



![image-20250119223727287](C:\Users\vgoer\AppData\Roaming\Typora\typora-user-images\image-20250119223727287.png)



### 1. 安装

> 编译安装

```shell
sudo apt install -y libpcap-dev golang git
git clone https://github.com/hktalent/scan4all.git
cd scan4all
go build

# 其他系统 你必须先安装libpcap库
# ubuntu、linux
apt update
apt install -yy libpcap0.8-dev
sudo apt install -y libpcap-dev
# cent os
yum install -yy glibc-devel.x86_64
yum install -yy libpcap
# mac os
brew install libpcap

# 直接安装
go install github.com/GhostTroops/scan4all@2.8.9
scan4all -h
```

> 版本： [release](https://github.com/GhostTroops/scan4all/releases)







### 2. 使用

> 使用


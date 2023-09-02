<center>kali</center>

[toc]



## kali

> 想用下debian系，所以选择在kali下开发



### 1.虚拟机

> 最大的虚拟机商：vmware 和 Virtualbox
>
> > vmare:[vm](https://www.vmware.com/cn/products/workstation-pro.html),收费的，功能强大，体验好
> >
> > virtualbox:[vxbox](https://www.virtualbox.org/),免费的，占用资源小  (下载拓展包导入)



###  2.kali

> Kali Linux 是一个基于 Debian 的开源 Linux 发行版，
>
> 面向各种信息安全任务，例如渗透测试、安全研究、计算机取证和逆向工程。

> 直接去官网[kali](https://www.kali.org)下载你想要的类型
>
> [parrot os ](https://www.parrotsec.org/)也是专注渗透的linux系统

```
默认密码  kail kali
修改密码
sudo passwd root
```



### 3.修改源

> 默认下载软件都会去官方的软件商店
>
> > 修改为国内的源

```shell
// 修改源
中科大源  清华源 阿里源 看你自己的地理位置

1. 复制
sudo cp /etc/apt/sourch.list /etc/apt/sourch.list.bak

2. 添加源(去源商店添加)
deb https://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src https://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib

3. 升级索引和系统
sudo apt-get update && sudo apt-get upgread -y
```



### 4.其他

```shell
1.输入法

apt-get install fcitx 
apt-get install fcitx-googlepinyin

```
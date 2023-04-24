---
title: Tor
description: 暗网系列
published: 1
date: 2023-04-24T10:27:52.850Z
tags: app, tor
editor: markdown
dateCreated: 2023-04-19T15:43:58.486Z
---

<center>free</center>

[toc]



## 科学上网

> 为了free



> 目前只是arch系的(方便)
>
> 其他linux可以去官网或github下载`appimage`文件



### 1.下载

> 可以下载`shadowsocks-qt `或 `v2Ray`

```shel
sudo pacman -Syyu  //升级更新系统

sudo pacman -Ss  shadowsocks-qt5 //查询有无软件

sudo pacman -S shadowsocks-qt5 //安装 ss服务代理
```

```shell
sudo yay -S v2ray-desktop  //安装 代理协议多
```

[v2ray](https://github.com/233boy/v2ray/releases) [soadowsocks](https://github.com/shadowsocks/ShadowsocksX-NG/releases)



### 2. 免费代理

> github有很多的

1. [free](https://github.com/freefq/free) 

2. [节点](https://docs.qq.com/doc/DZHlKc3BnVG1Rc2Zr)

3. [share](https://github.com/selierlin/Share-SSR-V2ray)

4. [appimag](https://github.com/selierlin/Share-SSR-V2ray/blob/master/SS/6-linux-setup-guide-cn.md)



### 3. 代理

把免费的节点导入到ss-qt中，设置**本地端口**



系统代理：  

> 这样是全局代理

* 网络设置  -- 代理
* 一般手动配置代理服务器  -- 输入（本地）端口和ip  



命令行代理：

> 只代理命令行打开的软件

```shell
// 1.下载  proxychains-ng 
sudo pacman -S proxychains-ng  

// 2.配置文件 /etc/proxychains.confg

[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
#socks4         127.0.0.1 9050
socks5 127.0.0.1 6666
// 最后面添加 
socks5 127.0.0.1 端口

// 打开代理
proxychains forefox //看见有日志走  而且是ok 加上 ss-qt 流量走 说明成功了
```

推荐一下：

浏览器插件： 

> 1. duckduckgo 
> 2. hackbar
> 3. 沙拉查词
> 4. dark reader   保护眼睛



### 4. tor

> 进入××网

```shell
//1. 下载tor
sudo pacaman -S tor 

//2. 打开 命令行
tor-browser  

// 3.不用代理一般情况很慢   设置代理
设置 --  tor  -- 最下面网络 
设置 proxy Typr:socks5
设置地址和端口  127.0.0.1 6666
```



> ok 啦 祝你上网愉快，嘿嘿，<小别致，挺东西>


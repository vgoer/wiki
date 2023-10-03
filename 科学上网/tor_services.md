---
title: tor_services
description: 
published: 1
date: 2023-06-27T10:48:35.855Z
tags: 
editor: markdown
dateCreated: 2023-06-27T10:48:34.401Z
---

<center>tor service</center>





[toc]





## tor service

> 搭建tor 服务, 让你的服务更加安全匿名







### 1. 下载tor客户端

> 洋葱浏览器：[tor](https://www.torproject.org/) [github](https://github.com/torproject)



```shell
# 1. 本机设置好全局代理

2. 设置中选中tor内置网桥 -- snowflake网桥 

3. 等待，因为tor是由开发者提供自己网络的，而且需要经过多层代理。所以访问是比较慢的

4. 还tm是 等待
```





### 2. 搭建tor服务

> [link](https://hostalk.net/posts/tor_onion.html)   [kk](https://ssrshare.github.io/2020/02/27/darkweb-webbuild/)



```shell
# 安装
sudo apt install tor


# 修改配置文件
sudo vim /etc/tor/torrc

#密钥和域名路径
HiddenServiceDir /var/lib/tor/hidden_service/
#下面这行是开启最新v3版域名，默认是v2版，去掉前面#号即可开启，v3域名更长
HiddenServiceVersion 3
#下面是默认端口转发，如果服务器不干别的话，俺是建议直接用80端口的
HiddenServicePort 80 127.0.0.1:8080


# 重启服务
sudo systemctl restart tor

# 查看tor目录    查看域名
cat /var/lib/tor/hidden_service/hostname 

# 牛皮 
aacc........dsfdsfasfsdafdsaf.onion
```



#### 3. 配置nginx

> 安装网络服务器

```shell
# 安装
sudo apt install nginx

# 配置 配置到我们tor配置的端口转发

vim /etc/nginx/nginx.conf
#server段添加下面内容

    server {
        listen 8080;    #可以直接改用80端口
        listen [::]:8080;
        root /var/www/tor; #网站根目录，这里就是你的网站内容
    }
    
# 重启
sudo systemctl restart nginx

# 在 /var/www/tor下创建web
```



> 访问： 记得用tor浏览器，不要用google之类的浏览器  
>
> > 我去。牛皮牛皮








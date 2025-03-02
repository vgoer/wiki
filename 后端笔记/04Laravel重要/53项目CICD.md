<center>项目CI/CD</center>







[toc]









## CI/CD

> 自动化构建部署。





### 1. 安装面板

> 宝塔面板： [bt](https://www.bt.cn/new/index.html)

```shell
if [ -f /usr/bin/curl ];then curl -sSO https://download.bt.cn/install/install_panel.sh;else wget -O install_panel.sh https://download.bt.cn/install/install_panel.sh;fi;bash install_panel.sh ed8484bec
```

> 记住`用户和密码`



### 2. 安装Jenkins

> Jenkins: 支持构建、部署、自动化， 满足任何项目的需要。[jenkins](https://www.jenkins.io/zh/)
>
> 宝塔面板上安装。

```shell
bt-> docker -> jenkins -> 安装

#详情可以看密码
jenkins密钥

```

> `tips`解决Docker镜像拉取失败
>
> 或者： ==宝塔Docker设置开启URL加速。==

Linux 系统：/etc/docker/daemon.json
Windows 系统：C:\\ProgramData\\docker\\config\\daemon.json
macOS 系统：~/Library/Containers/com.docker.docker/Data/docker-daemon.json

```shell
{
  "registry-mirrors": [
    "https://docker.hpcloud.cloud",
    "https://docker.m.daocloud.io",
    "https://docker.unsee.tech",
    "https://docker.1panel.live",
    "http://mirrors.ustc.edu.cn",
    "https://docker.chenby.cn",
    "http://mirror.azure.cn",
    "https://dockerpull.org",
    "https://dockerhub.icu",
    "https://hub.rat.dev"
  ]
}
```

```shell
# 重启
sudo systemctl daemon-reload
sudo systemctl restart docker
```





### 3. 创建远程仓库

> 用gitee或者github

![image-20250302113339012](.\assets\image-20250302113339012-17408864227871.png)

​	

<center>部署webman服务</center>





[toc]









## 部署webman服务

> 启动项目。webman服务。







### 1. 前置条件

> 1. 获取到群里打包的docker镜像。
> 2. 安装好docker和docker-compose







### 2. 步骤

```shell
# 1. 导入镜像
# 导入容器 容器名和标签 xdly/webman:8.1
docker import xdly_webman_docker.tar xdly/webman:8.1

# 2. 启动项目 确保项目代码路径
# 有webman项目 确定路径/home/goer/docker_data/webman
docker run -itd -p 8787:8787 -v /home/goer/docker_data/webman:/app --name webman xdly/webman:8.1

# 3. 代码配置了多环境。需要进入容器启动，默认读 .env文件。
# 进入容器
docker exec -it webman bash

# 开发环境启动
php start.php restart -e APP_ENV=dev -d

# 测试服务器
php start.php restart -e APP_ENV=test -d

# 生成环境
php start.php restart -e APP_ENV=pro -d
```


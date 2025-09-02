<center>搭建MogoDB</center>



[toc]







## 搭建MogoDB

> 搭建MogoDB 。









### 1. 安装

```shell
# 文件
sudo mkdir -p ./mogo
sudo chown -R 1001:1001 ./mogo
sudo chmod -R 777 ./mogo
```

> docker-compose.yaml

```shell
services:
  # MongoDB 服务
  mongodb:
    image: mongo:latest # 使用最新的 MongoDB 官方镜像
    container_name: mongodb
    ports:
      # 将容器内部的 27017 端口映射到宿主机的 27017 端口 (MongoDB 默认端口)
      - 27017:27017
    volumes:
      # 将命名卷 'mongodb_data' 挂载到容器内部 MongoDB 存储数据的目录
      - ./mongodb_data:/data/db
    restart: unless-stopped # 容器退出时总是重启，除非手动停止
    # 可选：如果您的应用或其他服务需要连接到 MongoDB，可以将它们连接到同一个网络
    # networks:
    #   - your_app_network # 替换为您的应用网络名称

```

> 启动： 

```shell
docker-compose up -d
```









### 2. 测试

> navicat连接mogo

```shell
use mydatabase

db.mycollection.insertOne({ name: "test", value: 1 })

db.mycollection.find()
```

> 就可以看到我们的数据库。
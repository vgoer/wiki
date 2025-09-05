<center>Crawlab分布式爬虫</center>



[toc]







## Crawlab分布式爬虫

> Crawlab: 分布式爬虫管理平台，支持任何语言和框架
>
> [github ](https://github.com/crawlab-team/crawlab)[doc](https://docs.crawlab.cn/en)









### 1. 安装

> docker

```shell
mkdir crawlab
sudo chown -R 1001:1001 ./crawlab
sudo chmod -R 777 ./crawlab

```

```yaml
services:
  master: 
    image: crawlabteam/crawlab:latest
    container_name: crawlab_example_master
    environment:
      CRAWLAB_NODE_MASTER: "Y"
      CRAWLAB_MONGO_HOST: "mongo"
    volumes:
      - "./.crawlab/master:/root/.crawlab"
    ports:    
      - 8080:8080
    depends_on:
      - mongo

  worker01: 
    image: crawlabteam/crawlab:latest
    container_name: crawlab_example_worker01
    environment:
      CRAWLAB_NODE_MASTER: "N"
      CRAWLAB_GRPC_ADDRESS: "master"
      CRAWLAB_FS_FILER_URL: "http://master:8080/api/filer"
    volumes:
      - "./.crawlab/worker01:/root/.crawlab"
    depends_on:
      - master

  worker02: 
    image: crawlabteam/crawlab:latest
    container_name: crawlab_example_worker02
    environment:
      CRAWLAB_NODE_MASTER: "N"
      CRAWLAB_GRPC_ADDRESS: "master"
      CRAWLAB_FS_FILER_URL: "http://master:8080/api/filer"
    volumes:
      - "./.crawlab/worker02:/root/.crawlab"
    depends_on:
      - master

  mongo:
    image: mongo:latest
    container_name: crawlab_example_mongo
```

```shell
docker-compose up -d
```

> 打开： http://127.0.0.1:8080/
>
> 账号: admin/admin
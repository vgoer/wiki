<center>PostgreSQL</center>





[toc]









## PostgreSQL

> PostgreSQL：世界上最先进的开源关系数据库 [sql](https://www.postgresql.org/)







### 1. docker 安装

```shell
sudo mkdir -p ./postgres
sudo chown -R 1001:1001 ./postgres
sudo chmod -R 777 ./postgres

docker pull postgres:latest

docker run -d \
  --name pg \
  -e POSTGRES_PASSWORD=mysecretpassword \
  -p 5432:5432 \
  -v ./postgres/data:/var/lib/postgresql/data \
  postgres:latest
```

> 进入容器

```shell
docker exec -it pg bash
psql -U postgres
```

> navicat连接pg。

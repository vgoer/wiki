<center>Docker+Hyper</center>









[toc]









## Docker+Hyper

> Hyper + Mysql + Redis + Phpmyadmin









### 1. yaml文件

> `docker-compose.yaml`

```yaml
services:
  hyperf:
    image: hyperf/hyperf:8.1-alpine-v3.18-swoole
    container_name: hyperf
    working_dir: /data/project
    volumes:
      - ./hyperf-skeleton:/data/project
    ports:
      - "9501:9501"
    environment:
      - TZ=Asia/Shanghai
    extra_hosts:
      - "host.docker.internal:host-gateway"
    depends_on:
      - mysql
      - redis
    networks:
      - app-network

  mysql:
    image: mysql:8.0
    container_name: mysql
    environment:
      MYSQL_ROOT_PASSWORD: hyperf_root_password
      MYSQL_DATABASE: hyperf_db
      MYSQL_USER: hyperf_user
      MYSQL_PASSWORD: hyperf_password
    ports:
      - "9502:3306"
    volumes:
      - ./mysql-data:/var/lib/mysql
    networks:
      - app-network

  redis:
    image: redis:7.0.5
    container_name: redis
    ports:
      - "9503:6379"
    volumes:
      - ./redis-data:/data
    networks:
      - app-network

  phpmyadmin:
    image: phpmyadmin:5.2
    container_name: phpmyadmin
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
      PMA_ARBITRARY: 1
      PMA_PASSWORD: "root_password"
    ports:
      - "9504:80"
    depends_on:
      - mysql
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
```

> 启动

```shell
docker-compose up -d
```


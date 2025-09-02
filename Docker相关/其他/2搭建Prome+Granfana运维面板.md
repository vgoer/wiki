<center>Prome+Granfana运维面板</center>



[toc]









## Prome+Granfana运维面板

> Prome+Granfana运维面板 [blog](https://blog.lcayun.com/3210.html)







### 1. 文件

```shell
sudo mkdir -p ./gran
sudo chown -R 1001:1001 ./gran
sudo chmod -R 777 ./gran
```

> 添加文件`prometheus.yml`

```shell
# prometheus.yml
global:
  scrape_interval: 15s # 默认的抓取间隔

scrape_configs:
  - job_name: 'prometheus' # 监控 Prometheus 自身
    # metrics_path defaults to /metrics
    # scheme defaults to http
    static_configs:
      # 'prometheus' 是 Docker Compose 服务名称
      # Prometheus 在容器内部的默认端口是 9090
      - targets: ['prometheus:9090']

```

> 添加文件： `docker-compose.yaml`

```shell
services:
  # Prometheus 服务
  prometheus:
    image: prom/prometheus:latest # 使用最新的 Prometheus 官方镜像
    container_name: prometheus
    # 映射 Prometheus 配置文件
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      # 持久化 Prometheus 数据到命名卷
      - ./prometheus_data:/prometheus
    ports:
      - 9090:9090
    command: # 启动命令，指定配置文件和数据目录
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
    restart: unless-stopped
    networks:
      - monitoring-network

  # Grafana 服务
  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - 3000:3000
    volumes:
      # 持久化 Grafana 数据到命名卷
      - ./grafana_data:/var/lib/grafana
      # 可选：如果您有自定义配置，可以将配置文件挂载进来
      # - ./grafana/grafana.ini:/etc/grafana/grafana.ini
    restart: unless-stopped # 容器退出时总是重启，除非手动停止
    networks:
      - monitoring-network # 连接到同一个自定义监控网络
    depends_on:
      - prometheus # 确保 Prometheus 启动后再启动 Grafana

# 定义自定义网络，方便容器间通信
networks:
  monitoring-network:
    driver: bridge

```

> 启动

```shell
docker-compose up -d
```







### 2. 测试

> 访问： 

```shell
# 1. Grafana  账号密码 admin/admin
http://127.0.0.1:3000/

# 2. Prometheus
http://127.0.0.1:9090/
```


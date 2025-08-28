<center>kafka</center>





[toc]







## kafka

> `Kafka` 是由 `Apache 软件基金会` 开发的一个开源流处理平台，由 `Scala` 和 `Java` 编写。该项目的目标是为处理实时数据提供一个统一、高吞吐、低延迟的平台。其持久化层本质上是一个 "按照分布式事务日志架构的大规模发布/订阅消息队列"
>
> [longlang/phpkafka](https://github.com/swoole/phpkafka) 组件由 [龙之言](http://longlang.org/) 提供，支持 `PHP-FPM` 和 `Swoole`。感谢 `Swoole 团队` 和 `禅道团队` 对社区做出的贡献。





### 1. 安装kafka

```shell
sudo mkdir -p ./data
sudo chown -R 1001:1001 ./data
sudo chmod -R 777 ./data
```

> 添加kafkaui配置文件

```shell
touch ./data/kafkaui/config.yml

vim ./data/kafkaui/config.yml
```

```yaml
   kafka:
     clusters:
       - name: local
         bootstrapServers: "kafka1:9091,kafka2:9092,kafka3:9093,kafka4:9094"

```

> 添加`docker-compose.yaml`文件

```yaml
services:
  zookeeper:
    image: bitnami/zookeeper:3.8
    container_name: zookeeper
    environment:
      - ALLOW_ANONYMOUS_LOGIN=yes
    ports:
      - "2181:2181"
    volumes:
      - ./data/zookeeper:/bitnami/zookeeper

  kafka1:
    image: bitnami/kafka:3.7
    container_name: kafka1
    depends_on:
      - zookeeper
    environment:
      - KAFKA_BROKER_ID=1
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181
      - KAFKA_CFG_LISTENERS=PLAINTEXT://:9091
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://kafka1:9091
      - ALLOW_PLAINTEXT_LISTENER=yes
    ports:
      - "9091:9091"
    volumes:
      - ./data/kafka1:/bitnami/kafka

  kafka2:
    image: bitnami/kafka:3.7
    container_name: kafka2
    depends_on:
      - zookeeper
    environment:
      - KAFKA_BROKER_ID=2
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181
      - KAFKA_CFG_LISTENERS=PLAINTEXT://:9092
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://kafka2:9092
      - ALLOW_PLAINTEXT_LISTENER=yes
    ports:
      - "9092:9092"
    volumes:
      - ./data/kafka2:/bitnami/kafka

  kafka3:
    image: bitnami/kafka:3.7
    container_name: kafka3
    depends_on:
      - zookeeper
    environment:
      - KAFKA_BROKER_ID=3
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181
      - KAFKA_CFG_LISTENERS=PLAINTEXT://:9093
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://kafka3:9093
      - ALLOW_PLAINTEXT_LISTENER=yes
    ports:
      - "9093:9093"
    volumes:
      - ./data/kafka3:/bitnami/kafka

  kafka4:
    image: bitnami/kafka:3.7
    container_name: kafka4
    depends_on:
      - zookeeper
    environment:
      - KAFKA_BROKER_ID=4
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181
      - KAFKA_CFG_LISTENERS=PLAINTEXT://:9094
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://kafka4:9094
      - ALLOW_PLAINTEXT_LISTENER=yes
    ports:
      - "9094:9094"
    volumes:
      - ./data/kafka4:/bitnami/kafka

  kafka-ui:
    container_name: kafka-ui
    image: provectuslabs/kafka-ui:latest
    ports:
      - "9000:8080"
    environment:
      DYNAMIC_CONFIG_ENABLED: 'true'
    volumes:
      - ./data/kafkaui/config.yml:/etc/kafkaui/dynamic_config.yaml
```

> 启动和访问： http://localhost:9000/

```shell
docker-compose up -d
```

> 牛皮牛皮。

> 一些报错：

```shell
1. Kafka 报错：NodeExistsException

这是因为 Zookeeper 里还残留着上一次 Kafka broker 的 session 信息，新的 broker 启动时 session id 不一致，导致注册失败。

sudo rm -rf ./data/zookeeper/*
sudo rm -rf ./data/kafka1/*
sudo rm -rf ./data/kafka2/*
sudo rm -rf ./data/kafka3/*
sudo rm -rf ./data/kafka4/*
```







### 2. 简单使用

> 简单使用。

#### 一、创建Topic（主题）

进入任意一个Kafka容器（如kafka1）：

```shell
docker exec -it kafka1 bash
```

创建一个分区数为4、副本数为3的topic（充分利用你的4台broker）：

```shell
kafka-topics.sh --create \
  --topic test-topic \
  --bootstrap-server kafka1:9091,kafka2:9092,kafka3:9093,kafka4:9094 \
  --partitions 4 \
  --replication-factor 3
```

参数说明：

- --partitions 4：分区数，建议和broker数一致或更多，利于高并发。

- --replication-factor 3：副本数，建议小于等于broker数，保证高可用。



#### 二、生产消息

在kafka1容器内执行：

```shell
kafka-console-producer.sh --topic test-topic --bootstrap-server kafka1:9091,kafka2:9092,kafka3:9093,kafka4:9094
```

> 生成消息中发送消息：消费消息可以看到。

#### 三、消费消息

可以在任意一个Kafka容器（如kafka2）执行：

```shell
docker exec -it kafka2 bash
kafka-console-consumer.sh --topic test-topic --from-beginning --bootstrap-server kafka1:9091,kafka2:9092,kafka3:9093,kafka4:9094
```


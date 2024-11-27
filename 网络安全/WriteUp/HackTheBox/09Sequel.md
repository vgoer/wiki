<center>Sequel</center>





[toc]







## Sequel

> Sequel







### 1. task

1. During our scan, which port do we find serving MySQL?

```shell
nmap -sV 10.129.95.232 -A

PORT     STATE SERVICE VERSION
3306/tcp open  mysql?
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.27-MariaDB-0+deb10u1
|   Thread ID: 135
|   Capabilities flags: 63486


3306
```

2. What community-developed MySQL version is the target running?

```shell
MariaDB
```

3. When using the MySQL command line client, what switch do we need to use in order to specify a login username?

```shell
-u
```

4. Which username allows us to log into this MariaDB instance without providing a password?

```shell
root
```

5. In SQL, what symbol can we use to specify within the query that we want to display everything inside a table?

```shell
*
```

6. In SQL, what symbol do we need to end each query with?

```shell
;
```

7. There are three databases in this MySQL instance that are common across all MySQL instances. What is the name of the fourth that's unique to this host?

```shell
mysql -h 10.129.95.232 -u root -p 

ERROR 2026 (HY000): TLS/SSL error: SSL is required, but the server does not support it
# 方法1：使用命令行参数
mysql -h 10.129.95.232 -u root --skip-ssl 

```





### 2. mysql

> mysql: MySQL 是最流行的开源关系型数据库管理系统

```shell
支持多种存储引擎
高性能
可靠性
易用性
跨平台
```

> `端口：3306`

> mysql 环境搭建

````shell
# 更新系统
sudo apt update
sudo apt upgrade

# 安装 MySQL
sudo apt install mysql-server mysql-client

# 安全配置
sudo mysql_secure_installation


#### 配置文件位置
```bash
# 主配置文件
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
````

```shell
[mysqld]
# 网络设置
bind-address = 127.0.0.1
port = 3306

# 字符集设置
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

# 存储设置
datadir = /var/lib/mysql

# InnoDB 设置
innodb_buffer_pool_size = 1G
innodb_log_file_size = 256M
innodb_flush_log_at_trx_commit = 1

# 连接设置
max_connections = 150
```

````shell

### 4. 用户和权限管理

```sql
-- 创建用户
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';

-- 授权
GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'localhost';
FLUSH PRIVILEGES;

-- 查看用户权限
SHOW GRANTS FOR 'username'@'localhost';
```


### 5. 数据库操作

```sql
-- 创建数据库
CREATE DATABASE dbname CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- 查看数据库
SHOW DATABASES;

-- 使用数据库
USE dbname;

-- 创建表
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```


### 6. 备份和恢复

```bash
# 备份单个数据库
mysqldump -u root -p database_name > backup.sql

# 备份所有数据库
mysqldump -u root -p --all-databases > all_databases.sql

# 恢复数据库
mysql -u root -p database_name < backup.sql
```

#### 配置优化
```ini:/etc/mysql/mysql.conf.d/mysqld.cnf
[mysqld]
# 缓冲池设置
innodb_buffer_pool_size = 4G
innodb_buffer_pool_instances = 4

# 日志设置
innodb_log_file_size = 512M
innodb_log_buffer_size = 16M

# 查询缓存
query_cache_size = 128M
query_cache_limit = 2M

# 临时表
tmp_table_size = 64M
max_heap_table_size = 64M
```

````







### 3. flag

> 获取flag



```shell
mysql -h 10.129.95.232 -u root --skip-ssl 

show databases;

use htb;

show tables;

select * from config;

5 | flag                  | 7b4bec00d1a39e3dd4e021ec3d915da8
```


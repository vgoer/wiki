<center>Mysql主从</center>





[toc]







## Mysql主从

> > [hyperf/database](https://github.com/hyperf/database) 衍生于 [illuminate/database](https://github.com/illuminate/database)，我们对它进行了一些改造，大部分功能保持了相同。在这里感谢一下 Laravel 开发组，实现了如此强大好用的 ORM 组件。
>
> [hyperf/database](https://github.com/hyperf/database) 组件是基于 [illuminate/database](https://github.com/illuminate/database) 衍生出来的组件，我们对它进行了一些改造，从设计上是允许用于其它 PHP-FPM 框架或基于 Swoole 的框架中的，而在 Hyperf 里就需要提一下 [hyperf/db-connection](https://github.com/hyperf/db-connection) 组件，它基于 [hyperf/pool](https://github.com/hyperf/pool) 实现了数据库连接池并对模型进行了新的抽象，以它作为桥梁，Hyperf 才能把数据库组件及事件组件接入进来。



### 1. 安装

```php
composer require hyperf/db-connection
composer require hyperf/database
```

> 缓存内里配置mysql





### 2. 主从配置

> ### [读写分离](https://hyperf.wiki/3.1/#/zh-cn/db/quick-start?id=读写分离)
>
> 有时候你希望 `SELECT` 语句使用一个数据库连接，而 `INSERT`，`UPDATE`，和 `DELETE` 语句使用另一个数据库连接。在 `Hyperf` 中，无论你是使用原生查询，查询构造器，或者是模型，都能轻松的实现

> 主从概念： 主从复制是mysql的一种数据冗余，提高数据库的高可用性。
>
> 1. 一主一从
> 2. 一主动从
> 3. 级联复制模式
> 4. 双复制模式

复制模式：

> 二进制复制模式： bin log复制模式。

> GTID复制模式： （目前常用）



1. 三台数据库

```php
主数据库： 3301
重数据库： 3302
重数据库： 3303
```

2. 拉取镜像

```php
docker pull mysql:8.0
```

3. 创建数据库

```php
// 主
docker run -d -p 3301:3306 --privileged=true -v ./mysql/master/log:/var/log/mysql -v ./mysql/master/data:/var/lib/mysql -v ./mysql/master/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=root --name mysql_master mysql:8.0
    
// 从1
docker run -d -p 3302:3306 --privileged=true -v ./mysql/slave1/log:/var/log/mysql -v ./mysql/slave1/data:/var/lib/mysql -v ./mysql/slave1/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=root --name mysql_slave1 mysql:8.0
    
// 从2
docker run -d -p 3303:3306 --privileged=true -v ./mysql/slave2/log:/var/log/mysql -v ./mysql/slave2/data:/var/lib/mysql -v ./mysql/slave2/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=root --name mysql_slave2 mysql:8.0
    
// 从3
docker run -d -p 3304:3306 --privileged=true -v ./mysql/slave3/log:/var/log/mysql -v ./mysql/slave3/data:/var/lib/mysql -v ./mysql/slave3/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=root --name mysql_slave3 mysql:8.0
```

> 配置主数据库

```php
sudo vim ./mysql/master/conf/my.cnf
```

```php
[mysqld]
# 基础配置
port=3306
pid-file       = /var/run/mysqld/mysqld.pid
socket         = /var/run/mysqld/mysqld.sock
datadir        = /var/lib/mysql
skip-name-resolve
symbolic-links=0
log-error = /var/log/mysqld.log

# 主从复制相关
server-id=1                # 主服务器唯一ID，不能和从库重复
log-bin=mall-mysql-bin     # 开启二进制日志，必须项
binlog-format=ROW          # 推荐使用ROW格式
gtid-mode=on      
enforce-gtid-consistency=on
log-slave-updates=1
skip-slave-start=1

default-time_zone='+8:00'   # 如需设置时区可取消注释

# 推荐追加配置
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci

# 性能与安全
innodb_file_per_table=1
innodb_flush_log_at_trx_commit=1
sync-binlog=1
max_connections=2000
max_allowed_packet=256M

# 二进制日志管理
expire_logs_days=7
max_binlog_size=100M

# 允许远程连接（如有安全需求可指定为实际IP）
bind-address=0.0.0.0

# 慢查询日志（可选，便于排查慢SQL）
slow_query_log=1
slow_query_log_file=/var/log/mysql-slow.log
long_query_time=2
```

```php
sudo vim ./mysql/slave1/conf/my.cnf
```

```php
[mysqld]
# 基础配置
port=3306
pid-file       = /var/run/mysqld/mysqld.pid
socket         = /var/run/mysqld/mysqld.sock
datadir        = /var/lib/mysql
skip-name-resolve
symbolic-links=0
log-error = /var/log/mysqld.log

# 主从复制相关
server-id=2                # 主服务器唯一ID，不能和从库重复
log-bin=mall-mysql-bin     # 开启二进制日志，必须项
binlog-format=ROW          # 推荐使用ROW格式
gtid-mode=on      
enforce-gtid-consistency=on
log-slave-updates=1
skip-slave-start=1

default-time_zone='+8:00'   # 如需设置时区可取消注释

# 推荐追加配置
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci

# 性能与安全
innodb_file_per_table=1
innodb_flush_log_at_trx_commit=1
sync-binlog=1
max_connections=2000
max_allowed_packet=256M

# 二进制日志管理
expire_logs_days=7
max_binlog_size=100M

# 允许远程连接（如有安全需求可指定为实际IP）
bind-address=0.0.0.0

# 慢查询日志（可选，便于排查慢SQL）
slow_query_log=1
slow_query_log_file=/var/log/mysql-slow.log
long_query_time=2
    
# 重要 slave设置为只读，只有super权限除外
read_only=1
```

```shell
sudo vim ./mysql/slave2/conf/my.cnf
```

```php
[mysqld]
# 基础配置
port=3306
pid-file       = /var/run/mysqld/mysqld.pid
socket         = /var/run/mysqld/mysqld.sock
datadir        = /var/lib/mysql
skip-name-resolve
symbolic-links=0
log-error = /var/log/mysqld.log

# 主从复制相关
server-id=3                # 主服务器唯一ID，不能和从库重复
log-bin=mall-mysql-bin     # 开启二进制日志，必须项
binlog-format=ROW          # 推荐使用ROW格式
gtid-mode=on      
enforce-gtid-consistency=on
log-slave-updates=1
skip-slave-start=1

default-time_zone='+8:00'   # 如需设置时区可取消注释

# 推荐追加配置
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci

# 性能与安全
innodb_file_per_table=1
innodb_flush_log_at_trx_commit=1
sync-binlog=1
max_connections=2000
max_allowed_packet=256M

# 二进制日志管理
expire_logs_days=7
max_binlog_size=100M

# 允许远程连接（如有安全需求可指定为实际IP）
bind-address=0.0.0.0

# 慢查询日志（可选，便于排查慢SQL）
slow_query_log=1
slow_query_log_file=/var/log/mysql-slow.log
long_query_time=2
    
# 重要 slave设置为只读，只有super权限除外
read_only=1
```

```shell
sudo vim ./mysql/slave3/conf/my.cnf
```

```shell
[mysqld]
# 基础配置
port=3306
pid-file       = /var/run/mysqld/mysqld.pid
socket         = /var/run/mysqld/mysqld.sock
datadir        = /var/lib/mysql
skip-name-resolve
symbolic-links=0
log-error = /var/log/mysqld.log

# 主从复制相关
server-id=4                # 主服务器唯一ID，不能和从库重复
log-bin=mall-mysql-bin     # 开启二进制日志，必须项
binlog-format=ROW          # 推荐使用ROW格式
gtid-mode=on      
enforce-gtid-consistency=on
log-slave-updates=1
skip-slave-start=1

default-time_zone='+8:00'   # 如需设置时区可取消注释

# 推荐追加配置
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci

# 性能与安全
innodb_file_per_table=1
innodb_flush_log_at_trx_commit=1
sync-binlog=1
max_connections=2000
max_allowed_packet=256M

# 二进制日志管理
expire_logs_days=7
max_binlog_size=100M

# 允许远程连接（如有安全需求可指定为实际IP）
bind-address=0.0.0.0

# 慢查询日志（可选，便于排查慢SQL）
slow_query_log=1
slow_query_log_file=/var/log/mysql-slow.log
long_query_time=2
    
# 重要 slave设置为只读，只有super权限除外
read_only=1
```

4. 进入主库创建

```shell
docker exec -it mysql_master  bash

# 链接
mysql -u root -p

# 主从创建用户授权
-- 创建slave用户和设置密码
CREATE USER 'slave'@'%' IDENTIFIED WITH mysql_native_password BY 'slave';

-- 授权slave
GRANT REPLICATION SLAVE, REPLICATION CLIENT on *.* to 'slave'@'%';


-- 修改密码 root可选
ALTER USER 'slave'@'%' IDENTIFIED WITH mysql_native_password BY 'slave';

-- root可选
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';

-- 刷新权限
flush privileges;

-- 查看
show master status;

-- 查看id
SHOW VARIABLES LIKE 'server_id';
```

5. 从库配置

```php
docker exec -it mysql_slave1  bash

# 链接
mysql -u root -p
    
# 创建只读用户
CREATE USER 'readuser'@'%' IDENTIFIED BY '123456';

-- 授予只读权限
GRANT SELECT on *.* to 'readuser'@'%';

-- 刷新权限
flush privileges;
```

6. 从库执行同步

```php
-- 1. mysql登录salve账户
show slave status\G;

-- 2.  执行同步
# 查看主库ip "IPAddress": "172.17.0.5",
docker inspect mysql_master 
    
-- 2.1 执行同步
change master to master_host='172.17.0.5',master_user='slave',master_password='slave',master_auto_position=1;

-- 3. 执行同步
start slave;

-- 4. 查看状态
show slave status\G;

-- 5. 确认无误
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
```

> 报错解决

```php
mysql> change master to master_host="172.17.0.5",master_user='slave',master_password='slave',master_auto_position=1;
ERROR 1777 (HY000): CHANGE REPLICATION SOURCE TO SOURCE_AUTO_POSITION = 1 cannot be executed because @@GLOBAL.GTID_MODE = OFF.
    
# GTID_MODE 没有打开
# 重启docker就好了
docker restart mysql_master
docker restart mysql_slave1
docker restart mysql_slave2
docker restart mysql_slave3
```

> navcat连接数据库： 连接添加数据。

```shell
1. 主服务器添加库，从也会添加。

2. 主添加表，从添加。

3. readuser 从服务器的只读用户不能写入表。
```

> 蛙去，牛皮。

> 最后 hyperf配置

```php
<?php

return [
    'default' => [
        'driver' => env('DB_DRIVER', 'mysql'),
        'read' => [
            'host' => ['192.168.1.1'],
        ],
        'write' => [
            'host' => ['196.168.1.2'],
        ],
        'sticky'    => true,
        'database' => env('DB_DATABASE', 'hyperf'),
        'username' => env('DB_USERNAME', 'root'),
        'password' => env('DB_PASSWORD', ''),
        'charset' => env('DB_CHARSET', 'utf8'),
        'collation' => env('DB_COLLATION', 'utf8_unicode_ci'),
        'prefix' => env('DB_PREFIX', ''),
        'pool' => [
            'min_connections' => 1,
            'max_connections' => 10,
            'connect_timeout' => 10.0,
            'wait_timeout' => 3.0,
            'heartbeat' => -1,
            'max_idle_time' => (float) env('DB_MAX_IDLE_TIME', 60),
        ],
    ],
];

```




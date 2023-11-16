<center>Mysql 主从同步</center>



[toc]







## 主从同步

> 问题： 在开发工作中，有时候会遇见某个`sql 语句`需要锁表，导致暂时不能使用读的服务，这样就会影响现有业务，使用主从复制，让主库负责写，从库负责读
>
> **主服务器负责写的操作，也就是增删改，从服务器负责读的操作**
>
> [blog1](https://blog.csdn.net/qq_38225558/article/details/121025633) [blog2](https://blog.csdn.net/qq_58804301/article/details/130061985) [juejing](https://juejin.cn/post/6931529095457013773)
>
> sql优化： [sql](https://learnku.com/articles/71177)





### 1. 流程

1. 主库db的更新事件(update、insert、delete)被写到binlog
2. 主库创建一个binlog dump thread，把binlog的内容发送到从库
3. 从库启动并发起连接，连接到主库
4. 从库启动之后，创建一个I/O线程，读取主库传过来的binlog内容并写入到relay log
5. 从库启动之后，创建一个SQL线程，从relay log里面读取内容，从Exec_Master_Log_Pos位置开始执行读取到的更新事件，将更新内容写入到slave的db

> 注：上述流程为相对流程，并非绝对流程







### 2. 搭建

> 可以看我的github仓库。这里有docker-compose搭建的教程



主从配置

```shell
# ================== ↓↓↓↓↓↓ 配置主库 ↓↓↓↓↓↓ ==================
# 进入主库
docker exec -it lar_mysql_master bash
# 登录mysql
mysql -uroot -proot123...
#  创建用户slave，密码123456
CREATE USER 'slave'@'%' IDENTIFIED BY '123456';
# 授予slave用户 `REPLICATION SLAVE`权限和`REPLICATION CLIENT`权限，用于在`主` `从` 数据库之间同步数据
GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'slave'@'%';
# 授予所有权限则执行命令: GRANT ALL PRIVILEGES ON *.* TO 'slave'@'%';
# 使操作生效
FLUSH PRIVILEGES;
# 查看状态
show master status;
# 注：File和Position字段的值slave中将会用到，在slave操作完成之前不要操作master，否则将会引起状态变化，即File和Position字段的值变化 !!!
# +------------------+----------+--------------+------------------+-------------------+
# | File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
# +------------------+----------+--------------+------------------+-------------------+
# | mysql-bin.000003 |      769 |              |                  |                   |
# +------------------+----------+--------------+------------------+-------------------+
# 1 row in set (0.00 sec)




# ================== ↓↓↓↓↓↓ 配置从库 ↓↓↓↓↓↓ ==================
# 进入从库
docker exec -it lar_mysql_slave bash
# 登录mysql
mysql -u root -proot123...

change master to master_host='www.zhengqingya.com',master_port=3306, master_user='slave', master_password='123456', master_log_file='mysql-bin.000003', master_log_pos= 769, master_connect_retry=30;
# 修改
CHANGE MASTER TO master_host='172.20.194.61', master_port=8011, master_user='slave', master_password='123456', master_log_file='mysql-bin.000011', master_log_pos=920, master_connect_retry=30;
# 执行成功 Query OK, 0 rows affected, 10 warnings (0.07 sec)
# 开启主从同步过程  【停止命令：stop slave;】
start slave;
# 查看主从同步状态
show slave status \G
# Slave_IO_Running 和 Slave_SQL_Running 都是Yes的话，就说明主从同步已经配置好了！
# 如果Slave_IO_Running为Connecting，SlaveSQLRunning为Yes，则说明配置有问题，这时候就要检查配置中哪一步出现问题了哦，可根据Last_IO_Error字段信息排错或谷歌…
# *************************** 1. row ***************************
#                Slave_IO_State: Waiting for master to send event
#                   Master_Host: www.zhengqingya.com
#                   Master_User: slave
#                   Master_Port: 3306
#                 Connect_Retry: 30
#               Master_Log_File: mysql-bin.000003
#           Read_Master_Log_Pos: 769
#                Relay_Log_File: c598d8402b43-relay-bin.000002
#                 Relay_Log_Pos: 320
#         Relay_Master_Log_File: mysql-bin.000003
#              Slave_IO_Running: Yes
#             Slave_SQL_Running: Yes
#               Replicate_Do_DB:

```

> 报错： `错误消息 "Authentication plugin 'caching_sha2_password' reported error: Authentication requires secure connection" 意味着 MySQL 服务器要求使用安全连接进行身份验证`

```shell
# 先登陆主数据库，用如下命令修改认证方式：
ALTER USER 'slave'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
```








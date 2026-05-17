# mysql-docker-replication
MySQL 8 GTID replication environment using Docker Compose

# MySQL 8 Docker GTID Replication Lab

# A production-style MySQL 8 replication environment using Docker Compose.

Features:
- GTID replication
- Semi-sync replication
- Persistent volumes
- Health check scripts
- Automatic replication initialization
- +-------------------+
- |---mysql-master----|
- |------:3306--------|
- +-------------------+
- ------|
- ------| GTID Replication
- ------|
- +-------------------+
- |---mysql-slave-----|
- |------:3306--------|
- +-------------------+
  
## Persistent Volumes

Data is stored in:

- ./master/data
- ./replica/data
  
## Stack
- Ubuntu 24.04
- Docker Engine 29
- Docker Compose
- MySQL 8.0

## Requirements

- Docker >= 29
- Docker Compose plugin
- 4GB RAM
- Linux host
  
## 部署启动

# 1 root@ubt:/opt/mysql-repl# docker compose up -d

[+] up 3/3

 ✔ Network mysql-repl_default Created                                                                                                                    0.2s
 
 ✔ Container mysql-master     Started                                                                                                                    1.6s
 
 ✔ Container mysql-slave      Started                                                                                                                    2.8s
 
# 2 root@ubt:/opt/mysql-repl# docker ps

CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                               NAMES 

ff194a360603   mysql:8.0.36   "docker-entrypoint.s…"   11 minutes ago   Up 11 minutes   33060/tcp, 0.0.0.0:3308->3306/tcp   mysql-slave 

48f1414eb7dd   mysql:8.0.36   "docker-entrypoint.s…"   11 minutes ago   Up 11 minutes   33060/tcp, 0.0.0.0:3307->3306/tcp   mysql-master

# 3 主库初始化 Create replication user

CREATE USER 'repl'@'%' IDENTIFIED BY 'repl123';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
FLUSH PRIVILEGES;

# 4 备库配置 Configure replica

CHANGE REPLICATION SOURCE TO
  SOURCE_HOST='mysql-master',
  SOURCE_USER='repl',
  SOURCE_PASSWORD='repl123',
  SOURCE_AUTO_POSITION=1;

# 4 备库 START REPLICA; 
SHOW REPLICA STATUS\G；

mysql> show replica status\G

           Replica_IO_Running: Yes
          Replica_SQL_Running: Yes
             Source_Server_Id: 1
                  Source_UUID: 36a76572-4f6f-11f1-8705-e2ade332269c
             Source_Info_File: mysql.slave_master_info
    Replica_SQL_Running_State: Replica has read all relay log; waiting for more updates
           Source_Retry_Count: 86400
           Retrieved_Gtid_Set: 36a76572-4f6f-11f1-8705-e2ade332269c:1
            Executed_Gtid_Set: 36a76572-4f6f-11f1-8705-e2ade332269c:1
                Auto_Position: 1

# 5 同步测试 主备库 数据
mysql> select * from lei.t1;
+------+
| id   |
+------+
|    1 |
|    2 |
+------+
2 rows in set (0.05 sec)

# 6 故障切换 Failover Test

1. Stop master
2. Promote replica
3. Reconfigure replication


  

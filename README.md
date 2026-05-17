# mysql-docker-replication
MySQL 8 GTID replication environment using Docker Compose

# MySQL 8 Docker GTID Replication Lab

# A production-style MySQL 8 replication environment using Docker Compose.

Features:
- GTID replication
- Semi-sync replication
- Persistent volumes
- Custom my.cnf
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
- 

## Stack
- Ubuntu 24.04
- Docker Engine 29
- Docker Compose
- MySQL 8.0

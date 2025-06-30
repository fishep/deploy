# 基于docker部署mysql
> 部署mysql8.0.32的集群模式, 一主多从, 适合读写分离

#### 部署 mysql-replication
```shell
docker compose -p dbs up -d 

# master开启同步
docker exec -it mysql-master bash

mysql -uroot -proot
create user 'mover'@'%' identified by 'mover';
grant replication slave on *.*  to 'mover'@'%';
flush privileges;

# slave开启同步
docker exec -it mysql-slave-1 bash

mysql -uroot -proot
SHOW MASTER STATUS;
SHOW SLAVE STATUS;
CHANGE MASTER TO MASTER_HOST='mysql-master', MASTER_USER='mover', MASTER_PASSWORD='mover', MASTER_AUTO_POSITION=1, MASTER_CONNECT_RETRY=3;
#CHANGE MASTER TO MASTER_HOST='mysql-master', MASTER_USER='mover', MASTER_PASSWORD='mover', MASTER_AUTO_POSITION=0, MASTER_LOG_FILE='e4c26551355e-bin.000003' ,MASTER_LOG_POS=3402, MASTER_CONNECT_RETRY=3;
START SLAVE;
#STOP SLAVE;

```

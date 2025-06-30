# 基于docker部署mysql
> 部署mysql8.0.32 的集群模式

#### 部署 mysql-cluster
```shell

docker compose -p dbs up -d 

# Run the SHOW command to print cluster status. 
docker exec -it management1 ndb_mgm
show

# 打印节点ip
docker exec -it alpine bash
for i in {1,2}; do ping -c 1 mysql$i; done | grep PING | sed -r "s/PING mysql[0-9] \((.*)\): 56 data bytes/\1/"  
```

# 基于docker部署seata服务
> seata-1.7.0

#### 部署
```shell

# 启动mysql，初始化数据库
docker cp ./init-mysql/seata-mysql-schema.sql mysql:/docker-entrypoint-initdb.d/
docker exec -it mysql bash
mysql -uroot -p"root" < /docker-entrypoint-initdb.d/seata-mysql-schema.sql

# 启动nacos，添加配置 
# 注意配置的group和namespace要和 ./config/seata/application.yml 配置的一致
# 复制 ./config/seata/seataServer.properties 到 nacos

docker compose -p ali up -d 

# http://localhost:7091 -- u/p: [seata/seata]

```
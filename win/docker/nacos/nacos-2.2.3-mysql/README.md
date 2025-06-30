# 基于docker部署nacos服务
> nacos-2.2.3 standalone-mysql 版本

#### standalone-mysql
```shell

# 需要先启动mysql服务，然后初始化数据库
docker cp ./init-mysql/nacos-mysql-schema.sql mysql:/docker-entrypoint-initdb.d/
docker exec -it mysql bash
mysql -uroot -p"root" < /docker-entrypoint-initdb.d/nacos-mysql-schema.sql

docker compose -p ali up -d 

# http://localhost:8848/nacos/ -- u/p: [nacos/nacos]

```

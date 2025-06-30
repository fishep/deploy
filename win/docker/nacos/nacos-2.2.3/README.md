# 基于docker部署nacos服务

#### standalone-derby
```shell

docker compose -p ali up -d 

# http://localhost:8848/nacos/ -- u/p: [nacos/nacos]

```

#### standalone-mysql
```shell

# 需要先启动mysql服务，然后初始化数据库
docker cp ./init-mysql/nacos-mysql-schema.sql mysql:/docker-entrypoint-initdb.d/
docker exec -it mysql bash
mysql -uroot -p"root" < /docker-entrypoint-initdb.d/nacos-mysql-schema.sql

docker compose -p ali up -d 

# http://localhost:8848/nacos/ -- u/p: [nacos/nacos]

```

#### cluster-embedded
```shell

# 需要先启动mysql服务，然后初始化数据库
docker cp ./init-mysql/nacos-mysql-schema.sql mysql:/docker-entrypoint-initdb.d/
docker exec -it mysql bash
mysql -uroot -p"root" < /docker-entrypoint-initdb.d/nacos-mysql-schema.sql

docker compose -p ali up -d 

```



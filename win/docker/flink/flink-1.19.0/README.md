# 基于docker部署flink服务
> flink-1.19.0

#### 部署flink
````shell
docker compose -p service up -d 

docker exec -it jobmanager bash 

flink run -c org.example.MyJob myFatJar.jar

````

#### 配置
```shell
# 容器里的配置是动态生成的
# 查看配置 conf/flink-conf.yaml ，配置到 docker-compose.yml 的环境变量去
```

#### 添加依赖
```shell
docker cp mysql-connector-j-8.0.32.jar jobmanager:/opt/flink/lib
docker cp flink-connector-jdbc-3.2.0-1.19.jar jobmanager:/opt/flink/lib
docker cp flink-shaded-hadoop-2-uber-2.8.3-10.0.jar jobmanager:/opt/flink/lib
docker cp paimon-flink-1.19-1.1-20250204.002533-45.jar jobmanager:/opt/flink/lib
docker cp flink-sql-connector-mysql-cdc-3.1.1.jar jobmanager:/opt/flink/lib
docker cp flink-doris-connector-1.19-1.6.1.jar jobmanager:/opt/flink/lib

docker exec -it jobmanager bash 
chown flink:flink ./lib/mysql-connector-j-8.0.32.jar
chown flink:flink ./lib/flink-connector-jdbc-3.2.0-1.19.jar
chown flink:flink ./lib/flink-shaded-hadoop-2-uber-2.8.3-10.0.jar
chown flink:flink ./lib/paimon-flink-1.19-1.1-20250204.002533-45.jar
chown flink:flink ./lib/flink-sql-connector-mysql-cdc-3.1.1.jar
chown flink:flink ./lib/flink-doris-connector-1.19-1.6.1.jar
chmod 644 ./lib/mysql-connector-j-8.0.32.jar
chmod 644 ./lib/flink-connector-jdbc-3.2.0-1.19.jar
chmod 644 ./lib/flink-shaded-hadoop-2-uber-2.8.3-10.0.jar
chmod 644 ./lib/paimon-flink-1.19-1.1-20250204.002533-45.jar
chmod 644 ./lib/flink-sql-connector-mysql-cdc-3.1.1.jar
chmod 644 ./lib/flink-doris-connector-1.19-1.6.1.jar

#重启 jobmanager taskmanager
docker compose -p service stop jobmanager taskmanager
docker compose -p service up -d

```

#### 启动 historyserver
```shell
# 查看配置 conf/flink-conf.yaml ，配置到 docker-compose.yml 的环境变量去
# HistoryServer

# 启动 historyserver
flink-daemon.sh start historyserver --configDir /opt/flink/conf

# flink webui  http://localhost:8081
# historyserver webui http://localhost:8082

```

#### flink sql
```shell
docker exec -it jobmanager bash
./bin/sql-client.sh

# 默认table，还可以设置为tableau、changelog
# SET sql-client.execution.result-mode=tableau;
# SET execution.runtime-mode=streaming; #默认streaming，也可以设置batch
# PRIMARY KEY (`id`) NOT ENFORCED
# 如果定义了主键，则连接器以upsert模式操作，否则，连接器以追加模式操作。

# test 1
DROP TABLE IF EXISTS `datagen_source`;
CREATE TABLE datagen_source (
    id INT,
    ts BIGINT,
    vc INT
)
WITH (
    'connector' = 'datagen',
    'rows-per-second'='1',
    'fields.id.kind'='random',
    'fields.id.min'='1',
    'fields.id.max'='10',
    'fields.ts.kind'='sequence',
    'fields.ts.start'='1',
    'fields.ts.end'='1000000',
    'fields.vc.kind'='random',
    'fields.vc.min'='1',
    'fields.vc.max'='100'
);

DROP TABLE IF EXISTS `datagen_sink`;
CREATE TABLE datagen_sink (
    id INT,
    ts BIGINT,
    vc INT
) WITH (
    'connector' = 'print'
);

INSERT INTO datagen_sink SELECT * FROM datagen_source;
#SELECT * FROM datagen_source;
#SELECT * FROM datagen_sink;
  
CREATE TABLE datagen_sum_sink (
    id INT,
    sumVC INT
) WITH (
    'connector' = 'print'
);

INSERT INTO datagen_sum_sink(id, sumVC)
SELECT id, SUM(vc) AS sumVC FROM datagen_source GROUP BY id;

#SELECT * FROM datagen_source;
#SELECT * FROM datagen_sum_sink;

# test 2
DROP TABLE IF EXISTS `mysql_flink_cdc_1`;
CREATE TABLE `mysql_flink_cdc_1`  (
  `id` BIGINT,
  `comment` STRING
) WITH (
   'connector' = 'jdbc',
   'url' = 'jdbc:mysql://mysql:3306/flink',
   'table-name' = 'flink_cdc_1',
   'username' = 'root',
   'password' = 'root'
);

DROP TABLE IF EXISTS `doris_flink_cdc_1`;
CREATE TABLE `doris_flink_cdc_1`  (
  `id` BIGINT,
  `comment` STRING
) WITH (
   'connector' = 'jdbc',
   'url' = 'jdbc:mysql://doris:9030/flink',
   'table-name' = 'flink_cdc_1',
   'username' = 'root',
   'password' = ''
);

SELECT * FROM `mysql_flink_cdc_1`;
SELECT * FROM `doris_flink_cdc_1`;

INSERT INTO `mysql_flink_cdc_1` (`id`, `comment`) VALUES (1, 'this is a test');
INSERT INTO `doris_flink_cdc_1` (`id`, `comment`) VALUES (2, 'this is a test');

INSERT INTO `doris_flink_cdc_1` (`id`, `comment`)
SELECT `id`, `comment` FROM `mysql_flink_cdc_1`;

```
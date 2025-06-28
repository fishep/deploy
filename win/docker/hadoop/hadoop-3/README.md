# 基于docker部署hadoop服务
> hadoop:3

#### 部署
````shell
docker compose -p hadoop up -d 
docker compose -p hadoop down -v

docker exec -it namenode bash 
docker exec -it datanode bash
docker exec -it resourcemanager bash
docker exec -it nodemanager bash

````

#### 测试
```shell
docker exec -it namenode bash

yarn jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar pi 10 15

hadoop fs --help
hadoop fs -ls /test
hadoop fs -mkdir /test
hadoop fs -chmod 777 /test
hadoop fs -rm -r -f /test

hdfs dfs --help
echo 'this is a test ' >  test.txt
hdfs dfs -put test.txt /test
hdfs dfs -ls /test


# cat etc/hadoop/core-site.xml
# cat etc/hadoop/hdfs-site.xml
# hdfs://namenode/test

```
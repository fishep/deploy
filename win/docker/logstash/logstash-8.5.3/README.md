# 基于docker部署logstash
> 部署 logstash-8.5.3

#### 部署
```shell
docker compose -p elk up -d 

docker exec -it logstash bash

```

#### test
```shell

# 执行 filebeat 的测试，往日志文件添加一条记录

# 在 kibana 里查看 elasticsearch 的索引是否增加

```

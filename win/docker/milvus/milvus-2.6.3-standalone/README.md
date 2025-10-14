# docker for milvus-2.6.3-standalone
> milvus-2.6.3-standalone

#### 部署
```shell

docker compose -p milvus up -d 

#docker exec -it milvus-etcd bash
docker exec -it milvus-minio bash
docker exec -it milvus-standalone bash

```

#### 查看
```shell

# 先配置 hosts
curl http://milvus-attu.win.local:8000

```



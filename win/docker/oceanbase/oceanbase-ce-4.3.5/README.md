# docker for oceanbase-ce-4.3.5
> oceanbase-ce-4.3.5

#### 部署
```shell

docker compose -p service up -d 

docker logs oceanbase | tail -1

docker exec -it oceanbase bash

```

#### 查看
```shell

# 先配置 hosts
curl http://oceanbase.win.local:2886

```

### 常用命令
```shell

docker exec -it oceanbase bash

#查看集群列表
obd cluster list
#查看 obcluster 集群详情
obd cluster display obcluster

#obclient -h127.0.0.1 -uroot@sys -A -Doceanbase -P2881 -p
obclient -h127.0.0.1 -uroot@sys -A -Doceanbase -P2881

```



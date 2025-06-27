# 基于docker部署filebeat
> 部署 filebeat-8.5.3

#### 部署
```shell
docker compose -p elk up -d 

docker exec -it filebeat bash

```

#### test
```shell
echo 'this is a test!' >> ./log/test.log
```

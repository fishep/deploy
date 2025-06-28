# 基于docker部署grafana
> 部署 grafana:10.0.3

#### 部署
```shell
docker compose -p pag up -d 

docker exec -it grafana sh

```

#### test
```shell

# http://localhost:3000/ -- u/p: [ admin / admin ]

```

#### 配置数据源
```shell

# http://prometheus:9090

```


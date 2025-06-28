# 基于docker部署prometheus
> 部署 prometheus-2.46.0

#### 部署
```shell
docker compose -p pag up -d 

docker exec -it prometheus sh
docker exec -it alertmanager sh
docker exec -it node-exporter sh

```

#### test
```shell

# prometheus
# http://localhost:9090/ -- u/p: [ / ]

# alertmanager
# http://localhost:9093/ -- u/p: [ / ]

# node-exporter
# http://localhost:9100/ -- u/p: [ / ]

```

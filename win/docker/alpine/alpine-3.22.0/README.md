# docker for alpine 3.22.0
> alpine 3.22.0

#### 部署
```shell

docker compose -p service up -d 

docker exec -it alpine sh

```

#### 安装工具
```shell
which curl

apk add curl

curl -V
```

#### Usage
```shell
FROM alpine:3.14
RUN apk add --no-cache mysql-client
ENTRYPOINT ["mysql"]
```


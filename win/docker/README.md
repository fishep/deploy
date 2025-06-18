# docker
> 在docker部署服务和应用

#### docker 常用指令
```shell
docker inspect redis | grep IPAddress
docker exec -it alpine bash

docker-compose -p service up -d

docker manifest create fishep/alpine:latest fishep/alpine:arm64 fishep/alpine:amd64
docker manifest push fishep/alpine:latest
docker manifest rm fishep/alpine:latest

docker run -it --rm --name=alpine --network=grace_doris_net fishep/alpine:latest bash
docker cp ./test.csv alpine:/opt

docker run -it --name=alpine fishep/alpine:amd64 bash
docker commit alpine fishep/alpine:amd64
docker push fishep/alpine:amd64

docker system prune

```

#### 使用自建的docker代理
```shell
# 配置docker
vim ~/.docker/config.json
#	"auths": {
#		"harbor.xx.com": {},
#		"https://index.docker.io/v1/": {}
#	},

docker login harbor.xx.com

# 查看代理可用仓库
curl https://registry.xx.com:6443/v2/_catalog

# 拉取镜像
docker pull harbor.xx.com/proxy/library/nginx:1.28.0

# 更改镜像名称
docker tag harbor.xx.com/proxy/library/nginx:1.28.0 nginx:1.28.0  # 修改仓库名和标签
docker rmi harbor.xx.com/proxy/library/nginx:1.28.0               # 删除旧名称

```

# deploy
本地开发环境部署

#### 本地域名规划
```shell
# 主机.域名
# 服务.主机.域名
# 服务.容器.主机.域名
# 服务.服务组.容器.主机.域名

local         # 局域网
mac.local     # 主机.域名
win.local     # 主机.域名
mysql.mac.local     # 服务.主机.域名
nginx.mac.local     # 服务.主机.域名
mysql.docker.mac.local     # 服务.容器.主机.域名
nginx.docker.mac.local     # 服务.容器.主机.域名
mysql.service.docker.mac.local     # 服务.服务组.容器.主机.域名
nginx.service.docker.mac.local     # 服务.服务组.容器.主机.域名

```

#### 目录规划
```shell
./      #local         # 局域网
./mac   #mac.local     # 主机.域名
./win   #win.local     # 主机.域名
./mac/mysql        #mysql.mac.local     # 服务.主机.域名
./mac/mysql/版本
./mac/nginx        #nginx.mac.local     # 服务.主机.域名
./mac/nginx/版本
./mac/docker/mysql        #mysql.docker.mac.local     # 服务.容器.主机.域名
./mac/docker/mysql/版本    #mysql.docker.mac.local     # 服务.容器.主机.域名
./mac/docker/nginx        #nginx.docker.mac.local     # 服务.容器.主机.域名
./mac/docker/nginx/版本    #nginx.docker.mac.local     # 服务.容器.主机.域名
./mac/docker/service      #*.service.docker.mac.local     # 服务.服务组.容器.主机.域名
./mac/docker/lamp         #*.lamp.docker.mac.local     # 服务.服务组.容器.主机.域名
./mac/docker/lnmp         #*.lnmp.docker.mac.local     # 服务.服务组.容器.主机.域名

```
# docker deploy nginx-1.28.0
> 在docker部署nginx

#### 修改配置
```shell

# 修改 volumes 的路径映射
vim docker-compose.yml

# 查看虚拟主机的配置
./etc/nginx/conf.d/

```

#### 部署
```shell
#docker run -it --rm \
#-v ./etc/nginx/nginx.conf:/etc/nginx/nginx.conf \
#-v ./etc/nginx/conf.d/:/etc/nginx/conf.d/ \
#-v ./etc/nginx/ssl/:/etc/nginx/ssl/ \
#-v ./www/project:/var/www/project \
#-p 80:80 \
#-p 443:443 \
#--name=nginx nginx:1.28.0

docker-compose -p service up -d

docker exec -it nginx bash
```

#### 测试
```shell
curl http://localhost/index.html

# 先配置 hosts
curl http://project.mac.local/index.html
curl http://project.mac.local/phpinfo.php

```

### 支持https
```shell
#openssl req -x509 -nodes -newkey rsa:2048 -keyout https.key -out https.crt -subj "/CN=localhost" -days 5000
#openssl req -x509 -nodes -newkey rsa:2048 -keyout https.key -out https.crt -days 9999
#openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout https.key -out https.crt
openssl req -x509 -nodes -days 365 -newkey rsa -keyout https.key -out https.crt

curl -k https://project.mac.local/index.html
curl -k https://project.mac.local/phpinfo.php

```


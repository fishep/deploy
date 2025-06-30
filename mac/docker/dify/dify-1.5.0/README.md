# 基于docker部署dify服务
> dify-1.5.0

#### 部署
```shell
#下载
git clone https://github.com/langgenius/dify.git

cd dify/docker
git checkout -b 1.5.0 1.5.0

cp .env.example .env
docker compose -p ai up -d

#安装
#http://localhost/install

#访问
#http://localhost

```

# 本地 部署 nginx

> 本地 部署 nginx, 最新稳定版

#### 安装

```shell
brew info nginx

brew install nginx
#Docroot is: /opt/homebrew/var/www
#The default port has been set in /opt/homebrew/etc/nginx/nginx.conf to 8080 so that
#nginx can run without sudo.
#nginx will load all files in /opt/homebrew/etc/nginx/servers/.

brew services info nginx
brew services run nginx
#brew services kill nginx
brew services start nginx # 开机自启
brew services stop nginx # 开机不自启

nginx -g daemon off
nginx
nginx -s stop
nginx -s reload
```

#### 配置虚拟主机
```shell

#openssl req -x509 -nodes -days 365 -newkey rsa -keyout https.key -out https.crt
#配置证书
cp ./etc/nginx/ssl /opt/homebrew/etc/nginx/

#配置虚拟主机
cp ./etc/nginx/servers/project.conf /opt/homebrew/etc/nginx/servers/

```

#### 测试
```shell
curl http://localhost/index.html

curl http://project.mac.local/index.html
curl http://project.mac.local/phpinfo.php

curl -k https://project.mac.local/index.html
curl -k https://project.mac.local/phpinfo.php

```

# docker
> 在docker部署php7.4

#### 修改配置
```shell

# 修改 volumes 的路径映射
vim docker-compose.yml

```

#### 部署php7.4
```shell

docker-compose -p service up -d

docker exec -it php bash

```

#### 触发 XDEBUG
```shell
# web
?XDEBUG_TRIGGER=PHPSTORM
#或
?XDEBUG_SESSION=PHPSTORM

# cli
# CLI执行时,没有HTTP Host信息，IDE不知道该用哪个Server, serverName需和IDE里的配置一致
export PHP_IDE_CONFIG="serverName=https.fs-manager.win.local"

export XDEBUG_TRIGGER=PHPSTORM
#或
export XDEBUG_SESSION=PHPSTORM
php myscript.php

```
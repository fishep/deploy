# docker
> 在docker部署php7.4

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
export XDEBUG_TRIGGER=PHPSTORM
#或
export XDEBUG_SESSION=PHPSTORM
php myscript.php

```
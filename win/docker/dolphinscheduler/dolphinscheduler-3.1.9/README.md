# 基于docker部署dolphinscheduler服务
> 3.1.9

#### deploy dolphinscheduler
```shell

# 如果需要初始化或者升级数据库结构，需要指定profile为schema
docker-compose -p dolphinscheduler --profile schema up -d

# 启动dolphinscheduler所有服务，指定profile为all
docker-compose -p dolphinscheduler --profile all up -d

# 进入容器
docker exec -it dolphinscheduler-postgresql bash
docker exec -it dolphinscheduler-zookeeper bash
docker exec -it dolphinscheduler-schema-initializer bash
docker exec -it dolphinscheduler-api bash
docker exec -it dolphinscheduler-alert bash
docker exec -it dolphinscheduler-master bash
docker exec -it dolphinscheduler-worker bash

```

#### 测试 dolphinscheduler
```shell
# http://localhost:12345/dolphinscheduler/ui -- u/p: [admin/dolphinscheduler123]
```

#### 添加用户
```shell
docker exec -it dolphinscheduler-worker bash
#useradd -r -s /usr/sbin/nologin fishep
useradd -m -s /usr/bin/bash fishep
passwd fishep
userdel -r fishep
```

#### mysql 数据源支持
```shell
docker cp mysql-connector-j-8.0.32.jar dolphinscheduler-api:/opt/dolphinscheduler/libs
docker cp mysql-connector-j-8.0.32.jar dolphinscheduler-alert:/opt/dolphinscheduler/libs
docker cp mysql-connector-j-8.0.32.jar dolphinscheduler-master:/opt/dolphinscheduler/libs
docker cp mysql-connector-j-8.0.32.jar dolphinscheduler-worker:/opt/dolphinscheduler/libs

#重启
docker-compose -p dolphinscheduler --profile all stop
docker-compose -p dolphinscheduler --profile all up -d
```

#### datax 支持
```shell
docker exec -it dolphinscheduler-worker bash
apt-get update
apt-get install python2.7
#apt-get remove python2.7
mkdir -p /opt/soft/python/bin && cd /opt/soft/python/bin
ln -s `which python2.7` python2.7

#下载 https://datax-opensource.oss-cn-hangzhou.aliyuncs.com/202309/datax.tar.gz
docker cp datax.tar.gz dolphinscheduler-worker:/opt/soft
tar -zxvf datax.tar.gz && rm datax.tar.gz
```

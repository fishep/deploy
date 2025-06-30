# 基于docker部署skywalking服务
> skywalking-9.2.0

#### deploy skywalking
```shell

docker compose -p apm up -d 

# http://localhost:8080/general -- u/p: [ / ]

```

#### 客户端
```shell

#下载agent https://skywalking.apache.org/downloads/

# 客户端程序配置，skywalking-agent.jar路径需更改
-DSW_AGENT_NAME=sk
-DSW_AGENT_COLLECTOR_BACKEND_SERVICES=skoap.apm.docker.mac.local:11800
-javaagent:./agent/skywalking-agent.jar

```
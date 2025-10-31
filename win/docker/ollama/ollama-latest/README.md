# 基于docker部署ollama服务
> ollama-latest

#### 部署
```shell

docker compose -p ai up -d

docker exec -it ollama bash

#默认是7b
ollama run deepseek-r1

ollama run llama3.1

#http://localhost:11434/

```

# docker deploy apisix-3.14.1
> 在docker部署apisix-3.14.1

#### 部署
```shell

git clone https://github.com/apache/apisix-docker.git
cd apisix-docker/example

docker-compose -p apisix -f docker-compose-arm64.yml up -d
docker-compose -p apisix -f docker-compose-arm64.yml down -v


docker exec -it apisix bash
docker exec -it etcd sh
docker exec -it web1 sh
docker exec -it web2 sh

```

#### 访问
```shell

curl "http://apisix.mac.local:9080" --head | grep Server

# ui
http://apisix.mac.local:9180/ui

```

#### 创建路由
```shell

curl -i "http://apisix.mac.local:9180/apisix/admin/routes" \
-H "X-API-KEY: edd1c9f034335f136f87ad84b625c8f1" \
-X PUT -d '
{
  "id": "getting-started-ip",
  "uri": "/ip",
  "upstream": {
    "type": "roundrobin",
    "nodes": {
      "httpbin.org:80": 1
    }
  }
}'

curl "http://apisix.mac.local:9080/ip"

```

#### 启用负载均衡
```shell

curl -i "http://apisix.mac.local:9180/apisix/admin/routes" \
-H "X-API-KEY: edd1c9f034335f136f87ad84b625c8f1" \
-X PUT -d '
{
  "id": "getting-started-headers",
  "uri": "/headers",
  "upstream" : {
    "type": "roundrobin",
    "nodes": {
      "httpbin.org:443": 1,
      "mock.api7.ai:443": 1
    },
    "pass_host": "node",
    "scheme": "https"
  }
}'

hc=$(seq 100 | xargs -I {} curl "http://apisix.mac.local:9080/headers" -sL | grep "httpbin" | wc -l); echo httpbin.org: $hc, mock.api7.ai: $((100 - $hc))

```

#### 密钥验证
```shell

curl -i "http://apisix.mac.local:9180/apisix/admin/consumers" \
-H "X-API-KEY: edd1c9f034335f136f87ad84b625c8f1" \
-X PUT -d '
{
  "username": "tom",
  "plugins": {
    "key-auth": {
      "key": "secret-key"
    }
  }
}'

curl -i "http://apisix.mac.local:9180/apisix/admin/routes/getting-started-ip" \
-H "X-API-KEY: edd1c9f034335f136f87ad84b625c8f1" \
-X PATCH -d '
{
  "plugins": {
    "key-auth": {}
  }
}'

curl -i "http://apisix.mac.local:9080/ip"
curl -i "http://apisix.mac.local:9080/ip" -H 'apikey: wrong-key'
curl -i "http://apisix.mac.local:9080/ip" -H 'apikey: secret-key'


#禁用 Authentication
curl "http://apisix.mac.local:9180/apisix/admin/routes/getting-started-ip" \
-H "X-API-KEY: edd1c9f034335f136f87ad84b625c8f1" \
-X PATCH -d '
{
  "plugins": {
    "key-auth": {
      "_meta": {
        "disable": true
      }
    }
  }
}'

curl -i "http://apisix.mac.local:9080/ip"

```

#### 限速
```shell

curl -i "http://apisix.mac.local:9180/apisix/admin/routes/getting-started-ip" \
-H "X-API-KEY: edd1c9f034335f136f87ad84b625c8f1" \
-X PATCH -d '
{
  "plugins": {
    "limit-count": {
        "count": 2,
        "time_window": 10,
        "rejected_code": 503
     }
  }
}'

count=$(seq 100 | xargs -I {} curl "http://apisix.mac.local:9080/ip" -I -sL | grep "503" | wc -l); echo \"200\": $((100 - $count)), \"503\": $count

#禁用 Rate Limiting
curl -i "http://apisix.mac.local:9180/apisix/admin/routes/getting-started-ip" \
-H "X-API-KEY: edd1c9f034335f136f87ad84b625c8f1" \
-X PATCH -d '
{
    "plugins": {
        "limit-count": {
            "_meta": {
                "disable": true
            }
        }
    }
}'

count=$(seq 100 | xargs -i curl "http://apisix.mac.local:9080/ip" -I -sL | grep "503" | wc -l); echo \"200\": $((100 - $count)), \"503\": $count


```
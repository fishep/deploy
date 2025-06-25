# docker 部署 kafka
> kafka-3.7.0

#### 部署
```shell

docker compose -p service up -d

```

#### 测试
````shell
docker exec -it kafka bash

# PLAINTEXT://kafka:9092
kafka-topics.sh --list --bootstrap-server kafka:9092
# EXTERNAL://kafka.service.docker.mac.local:9094
kafka-topics.sh --list --bootstrap-server kafka.service.docker.mac.local:9094

kafka-topics.sh --create --topic topic-demo --bootstrap-server kafka:9092
kafka-topics.sh --describe --topic topic-demo --bootstrap-server kafka:9092
kafka-topics.sh --alter --topic topic-demo --partitions 2 --bootstrap-server kafka:9092
kafka-topics.sh --delete --topic topic-demo --bootstrap-server kafka:9092

kafka-console-producer.sh --broker-list kafka:9092 --topic topic-demo
kafka-console-producer.sh --topic topic-demo --bootstrap-server kafka:9092
kafka-console-consumer.sh --topic topic-demo --from-beginning --bootstrap-server kafka:9092
kafka-console-consumer.sh --topic topic-demo --from-beginning --bootstrap-server kafka:9092 --group kafka-consumer-group

kafka-consumer-groups.sh --bootstrap-server kafka:9092 --list
kafka-consumer-groups.sh --bootstrap-server kafka:9092 --group kafka-consumer-group --describe

#-1 or latest / -2 or earliest / -3 or max-timestamp
kafka-run-class.sh org.apache.kafka.tools.GetOffsetShell --broker-list kafka:9092 --topic topic-demo
kafka-run-class.sh org.apache.kafka.tools.GetOffsetShell --broker-list kafka:9092 --topic topic-demo --time -1
kafka-run-class.sh org.apache.kafka.tools.GetOffsetShell --broker-list kafka:9092 --topic topic-demo --time -2
kafka-run-class.sh org.apache.kafka.tools.GetOffsetShell --broker-list kafka:9092 --topic topic-demo --time -3

kafka-configs.sh --bootstrap-server kafka:9092 --describe --entity-type topics | grep session.timeout.ms
kafka-configs.sh --bootstrap-server kafka:9092 --describe --entity-type topics | grep transaction.timeout.ms

cat << EOF > offset.json
{
    "partitions":[
        {
            "topic": "topic-demo",
            "partition": 0,
            "offset": 4
        }
    ],
    "version":1
}
EOF
kafka-delete-records.sh --offset-json-file offset.json --bootstrap-server kafka:9092

````

#### 运行示例
```shell
kafka-run-class.sh org.apache.kafka.streams.examples.wordcount.WordCountDemo
kafka-console-producer.sh --producer.config /opt/bitnami/kafka/config/producer.properties --bootstrap-server kafka:9092 --topic streams-plaintext-input
kafka-console-consumer.sh --consumer.config /opt/bitnami/kafka/config/consumer.properties --bootstrap-server kafka:9092 --topic streams-pipe-output --from-beginning
kafka-console-consumer.sh --consumer.config /opt/bitnami/kafka/config/consumer.properties --bootstrap-server kafka:9092 --topic streams-linesplit-output --from-beginning
kafka-console-consumer.sh --consumer.config /opt/bitnami/kafka/config/consumer.properties --bootstrap-server kafka:9092 --topic streams-wordcount-output --from-beginning
kafka-console-consumer.sh --bootstrap-server localhost:9092 \
    --topic streams-wordcount-output \
    --from-beginning \
    --formatter kafka.tools.DefaultMessageFormatter \
    --property print.key=true \
    --property print.value=true \
    --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
    --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
```
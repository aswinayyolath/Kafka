# Kafka Commands

## Start Zookeeper Server

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

## Start Kafka server

```bash
bin/kafka-server-start.sh config/server.properties
```

## Kraft mode

```bash
kafka-storage.sh random-uuid
```

```bash
kafka-storage.sh format -t <uuid> -c ../config/kraft/server.properties
```

```bash
kafka-server-start.sh ../config/kraft/server.properties
```


## Add config to topic

```bash
 bin/kafka-topics.sh --create --topic delete-this-topic --replication-factor 1 --partitions 1  --bootstrap-server localhost:9092 --config max.message.bytes=64000
```

## Alter and remove above added config

```bash
bin/kafka-topics.sh --alter --topic delete-this-topic  --zookeeper localhost:2181  --delete-config max.message.bytes
```

## Command to create a topic

```bash
bin/kafka-topics.sh --create --topic quickstart-events --replication-factor 1 --partitions 1  --bootstrap-server localhost:9092
```

## Describe Topic

```bash
bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
```

## Delete the topic

```bash
bin/kafka-topics.sh --delete --topic delete-this-topic --zookeeper localhost:2181
```

## Start Producer

```bash
bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
```

## Producer with Key

```bash
kafka-console-producer.sh --bootstrap-server localhost:9092 --topic new_topic --property "parse.key=true" --property "key.separator=:"
```

## Start Consumer - from befinning

```bash
bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```

## Describe Topic in Strmizi

```bash
 bin/kafka-topics.sh --describe --topic my-topic --bootstrap-server <BootStrap Server Address> --command-config /Users/aswina/Desktop/Strmizi/cli.config
 ```

## Consumer in Strmizi

```bash
bin/kafka-console-consumer.sh --bootstrap-server <BootStrap Server Address>  --consumer.config /Users/aswina/Desktop/Strmizi/cli.config --topic my-topic
```

## Performance Testing Producer

```bash
bin/kafka-producer-perf-test.sh \
--topic first-topic \
--num-records 1000 \
--record-size 1000 \
--throughput  1000 \
--producer-props \
bootstrap.servers=localhost:9092
```

## Performance Testing Consumer

```bash
 bin/kafka-consumer-perf-test.sh \
--broker-list localhost:9092 \
--topic first-topic \
--messages 10000
```

## Important Notes

- Once the message is consumed by a Consumer group then even if we use `--from-beginning` flag on console consumer it won't print the already consumer messages

```bash
bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092 --consumer.config config/consumer.properties
```

## Describe Consumer Group

```bash
./bin/kafka-consumer-groups.sh --describe  --bootstrap-server localhost:9092 --group test-consumer-group
```

## To run Kafka Source and Sink Connector

```bash
./bin/connect-standalone.sh config/connect-standalone.properties config/connect-console-source.properties config/connect-console-sink.properties
```

## Kafka Producer and consumer Conductor

```bash
kafka-console-producer.sh --producer.config  playground.config --bootstrap-server cluster.playground.cdkt.io:9092 --topic my_first_topic --producer-property partitioner.class=org.apache.kaf
ka.clients.producer.RoundRobinPartitioner
```

```bash
kafka-console-consumer.sh --consumer.config playground.config --bootstrap-server cluster.playground.cdkt.io:9092 --topic my_first_topic --group my_first_group
```

--from-beginning is an argument that is only helpful when there is never been a consumer offset has been comitted as part of the group
that is --from-beginning don't work if used with consumer group if the message is already read by the consumer.

## List Consumer group

```bash
kafka-consumer-groups.sh --command-config  playground.config --bootstrap-server cluster.playground.cdkt.io:9092 --list
```

## Describe Consumer group

```bash
kafka-consumer-groups.sh --command-config  playground.config --bootstrap-server cluster.playground.cdkt.io:9092 --describe --group my_first_group
```

## Dry run for resetting offset

```bash
kafka-consumer-groups.sh --command-config  playground.config --bootstrap-server cluster.playground.cdkt.io:9092 --group my_first_group --reset-offsets --to-earliest  --topic my_first_topic  --dry-run
```

## To actually reset offset

```bash
kafka-consumer-groups.sh --command-config  playground.config --bootstrap-server cluster.playground.cdkt.io:9092 --group my_first_group --reset-offsets --to-earliest  --topic my_first_topic  --execute
```

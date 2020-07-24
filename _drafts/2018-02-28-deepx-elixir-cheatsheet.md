---
layout: post
title: DeepX Elixir cheatsheet
categories: blog
intro:
---

## Running an application
1 - Up docker in background
Make sure you have the docker-compose.yml file descirbing our dockers.
```
docker-compose up -d
```


2 - Create topics used by the service
```
docker exec -it notificationservice_kafka_1 bash # runs the bassh in current docker isntance of kafka.

kafka-topics.sh --zookeeper zookeeper:2181 --create --partitions 50 --replication-factor 1 --topic user
kafka-topics.sh --zookeeper zookeeper:2181 --create --partitions 50 --replication-factor 1 --topic camp
kafka-topics.sh --zookeeper zookeeper:2181 --create --partitions 50 --replication-factor 1 --topic logging
kafka-topics.sh --zookeeper zookeeper:2181 --create --partitions 50 --replication-factor 1 --topic organization
kafka-topics.sh --zookeeper zookeeper:2181 --create --partitions 50 --replication-factor 1 --topic notification
```

3 - Setup database
```
mix ecto.setup # Setup is not a native ecto command, its an alias for ["ecto.create", "ecto.migrate"]
```

## Troubleshooting

kafka-console-producer.sh --property parse.key=true --property key.separator=, --broker-list localhost:9092 --topic user


1- Kafka console consumer/producer with a KEY:
```
# Producer
kafka-console-producer.sh --property parse.key=true --property key.separator=, --broker-list localhost:9092 --topic <<topic-name>>

#Consumer
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property key.separator=, --topic <<topic-name>>

#List all from beginning
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property key.separator=, --topic <<topic-name>> --from-beginning | grep "filter-string"


kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property key.separator=, --topic notfy --from-beginning | grep "filter-string"
```

2 - Create topics:

3 - Describe topics:
```
kafka-topics.sh --zookeeper zookeeper:2181 --describe --topic camp
```

Before push:
mix test --cover && mix credo -a

4 - Hierarchy of Messages:
`Topic -> Partition(n+) -> Msg (n+)`
Remeber that partitions are defined by a hash of its key.


Default value to function declaration

def bla(message, key // nil).

To create a new lib with an application.
mix new --sup naom-of-app

Kafka in Docker
===

This repository provides everything you need to run Kafka in Docker.

Run
---

```bash
docker run -d --name zookeeper boynux/kafka:0.10.1.0 bin/zookeeper-server-start.sh config/zookeeper.properties
docker run -d --name kafka --net container:zookeeper -p 9092:9092 --env ADVERTISED_HOST=0.0.0.0 --env ADVERTISED_PORT=9092 boynux/kafka:0.10.1.0 bin/kafka-server-start.sh config/server.properties
```

Create a Topic: 

    docker run -it --net container:kafka bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test

List Topics:

     docker run -it --net container:kafka bin/kafka-topics.sh --list --zookeeper localhost:2181

Start Producer:

    docker run -it --net container:kafka bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

Now switch to another terminal and start consumer:

    docker run -it --net container:kafka bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning

Now if you type in producer terminal you should be able to see the messages in consumer terminal!

Build from Source
---

    docker build -t kafka:0.10.1.0 -f kafka/Dockerfile kafka/

TODO
---

* Setup instruction for multiple instances
* Configuration options via Env variables

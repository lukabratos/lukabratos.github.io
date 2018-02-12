---
layout: post
title: "Hello World in Apache Kafka"
date: 2018-02-12 18:32:54 +0000
---
In this blog post, we will install Apache Kafka, create a topic, and send a few messages.

> [Kafka®](https://kafka.apache.org/) is used for building real-time data pipelines and streaming apps. It is horizontally scalable, fault-tolerant, wicked fast, and runs in production in thousands of companies.

# 1. Install Kafka

The easiest way to install Apache Kafka on macOS is by using [Homebrew](https://brew.sh/).

```bash
brew install kafka
```

# 2. Start the Zookeper and Kafka server

```bash
zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties & kafka-server-start /usr/local/etc/kafka/server.properties
```

> [Zookeeper](https://cwiki.apache.org/confluence/display/ZOOKEEPER/Index) is used to manage brokers in the Kafka cluster.

# 3. Create a topic

Kafka is architectured as distributed transaction log. This means that we can break down the data of each topic into multiple partitions which can be running on one or more servers all managed by the Kafka Cluster.
In this case, we're using one partition: `--partitions 1`.

For each topic, we can also set the `--replication-factor` where *n* is the number of servers. If we set *n* to 2, our topic's partitions log will be replicated on two servers *A* and *B*. If there is a failure on the server *A*, automatic failover to the server *B* will happen.

Let's create a topic named **hello-world**:

```bash
kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic hello-world
```

We should see this message: `Created topic "hello-world".` If you've created many different topics, you can view them by using the `list` command:

```bash
kafka-topics --list --zookeeper localhost:2181
```

# 4. Sending Messages

At its most basic principle, we use **Producers** to publish messages to the specific topics in the Kafka Cluster, and **Consumers** to subscribe to the specific topics and retrieve the messages.

## Producer

We can have an arbitrary number of processes called **producers**. When we execute the following command, we will start command line client that will be listening for any input. In my example, I'll be sending messages *a*, *b*, *c*, *d*, *e*, and *f* to the topic *hello-world*.

```bash
kafka-console-producer --broker-list localhost:9092 --topic hello-world
```

[![asciicast](https://asciinema.org/a/162493.png)](https://asciinema.org/a/162493)

## Consumer

Consumers role is to query the messages. We have published 6 messages so far to the topic *helo-world*. Let's see them:

```bash
kafka-console-consumer --bootstrap-server localhost:9092 --topic hello-world --from-beginning
```

Consumer client will now be listening for any new messages that might be published by the producer.
You can see that I'm using `--from-beginning` flag. It will make sure that messages from the beginning of the stream are retrieved.

[![asciicast](https://asciinema.org/a/162494.png)](https://asciinema.org/a/162494)

Happy message passing!
# commands for kafka

### link for kafka cluster setup
```bash
https://progressive-code.com/post/17/Setup-a-Kafka-cluster-with-3-nodes-on-CentOS-7
```

### secondary resource
```bash
https://www.digitalocean.com/community/tutorials/how-to-install-apache-kafka-on-centos-7
```

### create topic
```bash
bin/kafka-topics.sh --create --zookeeper kafka1:2181,kafka2:2181,kafka3:2181 --replication-factor 1 --partitions 6 --topic topic1 --config cleanup.policy=delete --config delete.retention.ms=60000
```

### describe topic 
```bash 
bin/kafka-topics.sh --zookeeper kafka2:2181 --describe --topic topic2
```

### list topics
```bash
kafka-topics --zookeeper zookeeper1:2181 --list
```

### determine the topics and replication settings for the kafka brokers
```bash
zkcli
ls /brokers/ids
ls /brokers/topics/topic1
ls /brokers/topics/topic/partitions
```

### re-balance cluster by forcing an election among the brokers for the primary replica
```bash
sudo kafka-preferred-replica-election --zookeeper kafka3:2181
```

### populate topic with data
```bash
seq 99 | /opt/kafka_2.12-2.2.0/bin/kafka-console-producer.sh --request-required-acks 1 --broker-list kafka1:9092 --topic numbers
```

### view live data
```bash
./kafka-console-consumer.sh --bootstrap-server kafka2:9092 --topic numbers --from-beginning --max-messages 99
```


###The following command line sends 10 million messages, 100 bytes in size, at a rate of 1000 messages/sec. The producer-props environment variable defines interceptors that enable stream monitoring in Control Center.
```bash
NS=io.confluent.monitoring.clients.interceptor && \ kafka-producer-perf-test \
        --topic i-love-logs \
        --num-records 10000000 \
        --record-size 100 \
        --throughput 1000 \
        --producer-props \
            bootstrap.servers=kafka-1:9092,kafka-2:9092 \
            interceptor.classes=${NS}.MonitoringProducerInterceptor
```

# getting kafka connect up and running

### resources
```bash
https://docs.confluent.io/current/connect/managing/confluent-hub/command-reference/confluent-hub-install.html

https://docs.confluent.io/current/connect/kafka-connect-syslog/index.html

https://docs.confluent.io/current/quickstart/cos-quickstart.html#cos-quickstart
```

###install kafka confluent on a node
```bash
cd /opt
tar xvf confluent-5.2.1-2.12.tar.gz
```

###install confluent hub client
```bash
https://docs.confluent.io/current/connect/managing/confluent-hub/client.html

tar xvf confluent-hub-client-latest.tar.gz

./bin/confluent-hub <enter rest of command here>
```

### installing kafka syslog connect
```bash
./bin/confluent-hub install confluentinc/kafka-connect-syslog:latest --component-dir /opt/confluent-5.2.1/share/confluent-hub-components --worker-configs /opt/confluent-5.2.1/etc/kafka/connect-standalone.properties
```

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

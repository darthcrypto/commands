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

# kafka health check

###describe a topic
```bash
./kafka-topics.sh --describe --zookeeper kafka1:2181 --topic syslogtest
```



# method fo streaming logs to kafka from rsyslog

###step 1: ensure on test server that /etc/hosts is updated with kafka servers

###step2: install filebeats

###step3: load the kafka module and system module for filebeats
```bash
filebeat modules enable system
filebeat modules enable kafka
filebeat modules list
```

###step4: use this filebeat.yml:
```bash
###################### Filebeat Configuration Example #########################

# This file is an example configuration file highlighting only the most common
# options. The filebeat.reference.yml file from the same directory contains all the
# supported options with more comments. You can use it as a reference.
#
# You can find the full configuration reference here:
# https://www.elastic.co/guide/en/beats/filebeat/index.html

# For more available modules and options, please see the filebeat.reference.yml sample
# configuration file.

#=========================== Filebeat inputs =============================

filebeat.inputs:

# Each - is an input. Most options can be set at the input level, so
# you can use different inputs for various configurations.
# Below are the input specific configurations.

- type: log

  # Change to true to enable this input configuration.
  enabled: true

  # Paths that should be crawled and fetched. Glob based paths.
  paths:
    - /var/log/*
    #- c:\programdata\elasticsearch\logs\*

  # Exclude lines. A list of regular expressions to match. It drops the lines that are
  # matching any regular expression from the list.
  #exclude_lines: ['^DBG']

  # Include lines. A list of regular expressions to match. It exports the lines that are
  # matching any regular expression from the list.
  #include_lines: ['^ERR', '^WARN']

  # Exclude files. A list of regular expressions to match. Filebeat drops the files that
  # are matching any regular expression from the list. By default, no files are dropped.
  #exclude_files: ['.gz$']

  # Optional additional fields. These fields can be freely picked
  # to add additional information to the crawled log files for filtering
  #fields:
  #  level: debug
  #  review: 1

  ### Multiline options

  # Multiline can be used for log messages spanning multiple lines. This is common
  # for Java Stack Traces or C-Line Continuation

  # The regexp Pattern that has to be matched. The example pattern matches all lines starting with [
  #multiline.pattern: ^\[

  # Defines if the pattern set under pattern should be negated or not. Default is false.
  #multiline.negate: false

  # Match can be set to "after" or "before". It is used to define if lines should be append to a pattern
  # that was (not) matched before or after or as long as a pattern is not matched based on negate.
  # Note: After is the equivalent to previous and before is the equivalent to to next in Logstash
  #multiline.match: after


#============================= Filebeat modules ===============================

filebeat.config.modules:
  # Glob pattern for configuration loading
  path: ${path.config}/modules.d/*.yml

  # Set to true to enable config reloading
  reload.enabled: false

  # Period on which files under path should be checked for changes
  #reload.period: 10s

#==================== Elasticsearch template setting ==========================

#setup.template.settings:
 # index.number_of_shards: 3
  #index.codec: best_compression
  #_source.enabled: false

#================================ General =====================================

# The name of the shipper that publishes the network data. It can be used to group
# all the transactions sent by a single shipper in the web interface.
#name:

# The tags of the shipper are included in their own field with each
# transaction published.
#tags: ["service-X", "web-tier"]

# Optional fields that you can specify to add additional information to the
# output.
#fields:
#  env: staging


#============================== Dashboards =====================================
# These settings control loading the sample dashboards to the Kibana index. Loading
# the dashboards is disabled by default and can be enabled either by setting the
# options here, or by using the `-setup` CLI flag or the `setup` command.
#setup.dashboards.enabled: false

# The URL from where to download the dashboards archive. By default this URL
# has a value which is computed based on the Beat name and version. For released
# versions, this URL points to the dashboard archive on the artifacts.elastic.co
# website.
#setup.dashboards.url:

#============================== Kibana =====================================

# Starting with Beats version 6.0.0, the dashboards are loaded via the Kibana API.
# This requires a Kibana endpoint configuration.
#setup.kibana:

  # Kibana Host
  # Scheme and port can be left out and will be set to the default (http and 5601)
  # In case you specify and additional path, the scheme is required: http://localhost:5601/path
  # IPv6 addresses should always be defined as: https://[2001:db8::1]:5601
  #host: "localhost:5601"

  # Kibana Space ID
  # ID of the Kibana Space into which the dashboards should be loaded. By default,
  # the Default Space will be used.
  #space.id:

#============================= Elastic Cloud ==================================

# These settings simplify using filebeat with the Elastic Cloud (https://cloud.elastic.co/).

# The cloud.id setting overwrites the `output.elasticsearch.hosts` and
# `setup.kibana.host` options.
# You can find the `cloud.id` in the Elastic Cloud web UI.
#cloud.id:

# The cloud.auth setting overwrites the `output.elasticsearch.username` and
# `output.elasticsearch.password` settings. The format is `<user>:<pass>`.
#cloud.auth:

#================================ Outputs =====================================

# Configure what output to use when sending the data collected by the beat.

#-------------------------- Elasticsearch output ------------------------------
#output.elasticsearch:
  # Array of hosts to connect to.
  #hosts: ["localhost:9200"]

  # Enabled ilm (beta) to use index lifecycle management instead daily indices.
  #ilm.enabled: false

  # Optional protocol and basic auth credentials.
  #protocol: "https"
  #username: "elastic"
  #password: "changeme"

#----------------------------- Logstash output --------------------------------
#output.logstash:
  # The Logstash hosts
  #hosts: ["localhost:5044"]

  # Optional SSL. By default is off.
  # List of root certificates for HTTPS server verifications
  #ssl.certificate_authorities: ["/etc/pki/root/ca.pem"]

  # Certificate for SSL client authentication
  #ssl.certificate: "/etc/pki/client/cert.pem"

  # Client Certificate Key
  #ssl.key: "/etc/pki/client/cert.key"

#================================ Processors =====================================

# Configure processors to enhance or manipulate events generated by the beat.

processors:
  - add_host_metadata: ~
  - add_cloud_metadata: ~

#================================ Logging =====================================

# Sets log level. The default log level is info.
# Available log levels are: error, warning, info, debug
#logging.level: debug

# At debug level, you can selectively enable logging only for some components.
# To enable all selectors use ["*"]. Examples of other selectors are "beat",
# "publish", "service".
#logging.selectors: ["*"]

#============================== Xpack Monitoring ===============================
# filebeat can export internal metrics to a central Elasticsearch monitoring
# cluster.  This requires xpack monitoring to be enabled in Elasticsearch.  The
# reporting is disabled by default.

# Set to true to enable the monitoring reporter.
#xpack.monitoring.enabled: false

# Uncomment to send the metrics to Elasticsearch. Most settings from the
# Elasticsearch output are accepted here as well. Any setting that is not set is
# automatically inherited from the Elasticsearch output configuration, so if you
# have the Elasticsearch output configured, you can simply uncomment the
# following line.
#xpack.monitoring.elasticsearch:


output.kafka:
  # specifying filebeat to take timestamp and message fields, other wise it
  # take the lines as json and publish to kafka

  # kafka
  # 10.252.94.61 is my local machine ip address
  # publishing to 'log' topic
  hosts: ["192.168.56.101:9092","192.168.56.102:9092","192.168.56.103:9092"]
  topic: "syslogtest"
```

###step 5, start filebeats:
```bash
systemctl start filebeat; systemctl enable filebeat
```

###step 6, get verbose output
```bash
filebeat -e
```

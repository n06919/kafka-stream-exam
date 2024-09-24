# kafka streams

### Start Server

```bash
# Generate a Cluster UUID
$ KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
$ echo $KAFKA_CLUSTER_ID
# Format Log Directories
$ ./bin/kafka-storage.sh format -t $KAFKA_CLUSTER_ID -c config/kraft/server.properties

# Start the Kafka Server
$ ./bin/kafka-server-start.sh config/kraft/server.properties

```

### Create Topic

```bash

# create topic: streams-plaintext-input
$ ./bin/kafka-topics.sh --create \
    --bootstrap-server localhost:9092 \
    --replication-factor 1 \
    --partitions 1 \
    --topic streams-plaintext-input

# create topic: streams-wordcount-output
$ ./bin/kafka-topics.sh --create \
    --bootstrap-server localhost:9092 \
    --replication-factor 1 \
    --partitions 1 \
    --topic streams-wordcount-output \
    --config cleanup.policy=compact
    

# describe topic: streams-plaintext-input
$ ./bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic streams-plaintext-input

# describe topic: streams-wordcount-output	
$ ./bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic streams-wordcount-output
```

### WordCountDemo

```bash
git clone https://github.com/n06919/kafka-stream-exam.git
export USER_DIR=/config/workspace
export CLASSPATH=$USER_DIR/kafka-stream-exam/build/classes/java/main:${CLASSPATH#:}
cd $USER_DIR/kafka-stream-exam/build/classes/java/main/com/skcc/college/streams
$ $USER_DIR/kafka_2.13-3.8.0/bin/kafka-run-class.sh com.skcc.college.streams.WordCountDemo

# 새창
# kafka home
$ cd /config/workspace/kafka_2.13-3.8.0
**$ ./bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic streams-plaintext-input
>all streams lead to kafka**

# 새창
# kafka home
$ cd /config/workspace/kafka_2.13-3.8.0
**$ ./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 \
    --topic streams-wordcount-output \
    --from-beginning \
    --property print.key=true \
    --property print.value=true \
    --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
    --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer

all	    1
streams	1
lead	  1
to	    1
kafka 	1

# producer
> hello kafka streams

# consumer
all	    1
streams	1
lead	  1
to	    1
kafka	  1
hello	  1
kafka	  2
streams	2**
```

### WordCountProcessorDemo

```bash
export USER_DIR=/config/workspace
export CLASSPATH=$USER_DIR/kafka-stream-exam/build/classes/java/main:${CLASSPATH#:}
cd $USER_DIR/kafka-stream-exam/build/classes/java/main/com/skcc/college/streams
$ $USER_DIR/kafka_2.13-3.8.0/bin/kafka-run-class.sh com.skcc.college.streams.WordCountProcessorDemo

----------- 1726823099750 ----------- 
[, 1]
[all, 1]
[hello, 4]
[kafka, 5]
[lead, 1]
[streams, 5]
[to, 1]

----------- 1726823220622 ----------- 
[, 1]
[all, 2]
[hello, 5]
[kafka, 7]
[lead, 2]
[streams, 7]
[to, 2]
```

### WordCountTransformerDemo

```bash
export USER_DIR=/config/workspace
export CLASSPATH=$USER_DIR/kafka-stream-exam/build/classes/java/main:${CLASSPATH#:}
cd $USER_DIR/kafka-stream-exam/build/classes/java/main/com/skcc/college/streams
$ $USER_DIR/kafka_2.13-3.8.0/bin/kafka-run-class.sh com.skcc.college.streams.WordCountTransformerDemo
----------- 1726823220622 ----------- 
[, 1]
[all, 2]
[hello, 5]
[kafka, 7]
[lead, 2]
[streams, 7]
[to, 2]
```
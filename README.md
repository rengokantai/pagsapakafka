# pagsapakafka
##2. Getting Started with Apache Kafka
###1 Enterprise Challenges with Data
Extract, Transform, and Load(ETL)


##4. Understanding Topics, Partitions, and Brokers
###4 Demo: Starting Apache Kafka and Producing and Consuming Messages
1st window:
```
cd /usr/local/bin/kafka
bin/zookeeper-server-start.sh config/zookeeper.properties
```
test connection(2nd window)
```
telnet localhost 2181
```
start kafka server(2nd window)
```
bin/kafka-server-start.sh config/server.properties
```
start a single kafka broker (3rd window)
```
bin/kafka-topics.sh  --create --topic ke --zookeeper localhost:2181 --replication-factor 1 --partitions 1
```
list topics(3rd window)
```
bin/kafka-topics.sh --list --zookeeper localhost:2181
```
produce messages(3rd window)
```
 bin/kafka-console-producer.sh  --broker-list localhost:9092 --topic ke
```
type anything.  
open 4th window.
```
bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic ke --from-beginning
```
check warning log
```
cd /tmp/kafka-logs/ke-0
``` 

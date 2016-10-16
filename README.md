# pagsapakafka
##2. Getting Started with Apache Kafka
###1 Enterprise Challenges with Data
Extract, Transform, and Load(ETL)


##4. Understanding Topics, Partitions, and Brokers
###4 Demo: Starting Apache Kafka and Producing and Consuming Messages
Must use 1GB+  
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
start a single kafka broker (3nd window)
```
bin/kafka-topics.sh  --create --topic ke --zookeeper localhost:2181 --replication-factor 1 --partitions 1
```

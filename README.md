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

###6 Apache Kafka Partitions in Detail
####kafka partitions  
each topic has one or more partitions  
a partition is the basis for which kafka can:
- scale
- become fault-tolerant
- Archieve higher levels of throughput

Each partition is maintained on at least one or more brokers.  

in log folder, we have
```
/tmp/kafka-logs/{topic}-{partition}   //like ke-0
```
###7 Distributed Partition Management in Apache Kafka
####Partitioning trade-offs
The more partitions the greater the zookeeper overhead.  
Message ordering can become complex 


###8 Achieving Reliability with Apache Kafka Replication
####Replication Factor
Reliable work distribution
- Redundency of messages
- Cluster resiliency
- Fault-tolerance  

gurantees
- N-1 borker failure tolerance
- 2 or 3 minimum
- per topic basis  


Hence, set to 3
```
bin/kafka-topics.sh  --create --topic ke --zookeeper localhost:2181 --replication-factor 3 --partitions 3
```
###9 Demo: Fault-tolerance and Resiliency in Apache Kafka
machine2,machine3, (need to change id, port, log dic)
```
bin/kafka-server-start.sh config/server1.properties
bin/kafka-server-start.sh config/server2.properties
```
machine4
```
bin/kafka-topics.sh  --create --topic ke_rep --zookeeper localhost:2181 --replication-factor 2 --partitions 1
```
then
```
bin/kafka-topics.sh --describe --topic ke_rep --zookeeper localhost:2181
```
producer:
```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic ke_rep
```
consumer:
```
bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic ke_rep --from-beginning
```
##5. Producing Messages with Kafka Producers
###2 Basics of Creating an Apache Kafka Producer
Properties
- bootstrap.servers
- key.serializer
- value.serializer

###9 Demo: Creating and Running an Apache Kafka Producer Application in Java
code
```
import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;

import java.util.Properties;

public Pro{
 public static void main(String[] args){
  Properties props = new Properties();
  props.put("bootstrap.servers","localhost:9092,localhost:9093");
  props.put("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
  props.put("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
  KafkaProducer<String,String,> ke = new KafkaProducer<String,String>(props);
  try{
   for(int i=0;i<100;i++){
    ke.send(new ProducerRecord<String,String>("topic",Integer.toString(i),"Message"+Integer.toString(i)));
   }
  }catch(Exception e){
  e.printStackTrace();
  }finally{
  ke.close();
  }
 }
}
```
##6. Consuming Messages with Kafka Consumers and Consumer Groups
###6 Demo: Simple Kafka Consumer
alter partition number of a topic
```
bin/kafka-topics.sh --zookeeper localhost:2181 --alter --topic ke --partitions 4
```
code
```
import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.common.TopicPartition;
import java.util.Properties;
import java.util.ArrayList;
public Con{
 public static void main(String[] args){
  Properties props = new Properties();
  props.put("bootstrap.servers","localhost:9092,localhost:9093");
  props.put("key.serializer","org.apache.kafka.common.serialization.StringSerializer");
  props.put("value.serializer","org.apache.kafka.common.serialization.StringSerializer");
  KafkaConsumer k = new KafkaConsumer(props);
  ArrayList<TopicPartition> par = new ArrayList<TopicPartition>();
  TopicPartition ke1 = new TopicPartition("ke2",0);
  TopicPartition ke2 = new TopicPartition("ke1",0);
  par.add(ke1);
  par.add(ke2);
  k.assign(par);
  try{
   while(true){
    ConsumerRecords<String,String> records = k.poll(10);
    for(ConsumerRecord<String,String> record:records){
     System.out.println(String.format("Topic: %s,Partition: %d, Offset: %d, Key: %s, Value: %s", record.topic(), record.partition(),record.offset(), record.key(),record.value()));
     }
    }
   }catch(Exception e){
   }finally{
    k.close();
   }
  }
 } 
```
benchmark
```
bin/kafka-producer-perf-test.sh --topic ke_rep --num-records 50 --record-size 1 --throughput 10 --producer-props bootstrap.servers=localhost:9092 key.serializer=org.apache.kafka.common.serialization.StringSerializer value.serializer=org.apache.kafka.common.serialization.StringSerializer
```

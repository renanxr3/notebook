
# Apache Kafka v3

Created by LinkedIn.

## Topics

* It is a particular stream of data
* Identified by its name
* topics are split in partitions
* messages within partitions are ordered (id is called offset)


## Messaging

Message Content:
* Key: will be used to determine partition, is optional (if null, data is sent round robin)
* Value: message content, simple byte array (no format itself, need to be serialized and deserialized by the applications)
* Compression type: none, gzip, snappy, lz4, zstd
* Headers
* Timestamp: event time or ingestion time, system or user generated

Broker: 
* is a Kafka instance
* runs in a speficic port
* Kafka cluster is composed of many brokers
* also called 'bootstrap server'

Logs
* physical files to store data (messages)
* rolling files
* pruned automatically (default is 7 days)

Producer:
* the applcation sending messages TO Kafka
* knows the partition to write to
* may be sync or assynchronous
* Default Key Hashing: murmur2 algorithm

Consumer:
* the application reading messages FROM kafka
* know which broker to read from
* in case of broker failure, consumer know how to recover
* reads in order, in the same partition
* may read from a specific offset or by the last commited offset

Consumer Group:
* is acceptable to have multiple consumer groups in the same topic
* consumer property 'group.id'
* Commits (delivery strategies)
  * At least once (usually preferred): 
    * offsets are commited after message is processed
    * If it goes wrong, message will be read again (duplicates)
  * At most once
    * offsets are commited as soon as msessages are delivered
    * if it goes wrong, some messages will be lost
    * Example use case: Real-time analytics, where occasional message loss is acceptable.
  * Exactly once
    * Kafka workflows: use the Transactional API
    * External systems workflows: use an idempotent consumer (refers to a consumer that can process the same message multiple times without producing unintended side effects or inconsistent results)
    * Example use case: Financial transactions, where precision and consistency are critical.


~~~
> [!NOTE]
> Consumer groups in Apache Kafka are incredibly useful for scaling and parallelizing data processing. Here's a real-world example:

>Imagine a streaming platform like Netflix that needs to process user activity logs in real-time to recommend content. These logs are sent to a Kafka topic. To handle the massive volume of data efficiently, multiple consumers can be grouped into a consumer group. Each consumer in the group processes data from a specific partition of the topic, ensuring no overlap. This setup allows the platform to process logs faster and provide personalized recommendations almost instantly.

Consumer groups also ensure fault tolerance. If one consumer fails, Kafka redistributes its workload to the remaining consumers in the group, maintaining seamless data processing.

Would you like to dive deeper into Kafka's architecture or explore another use case?
~~~

~~~
Great question, Renan! When you're using Apache Kafka and a consumer group, **you don’t need to explicitly specify which partitions a consumer should read from**. Kafka takes care of assigning partitions to consumers within the group automatically. 

This assignment is based on the number of consumers in the group and the number of partitions in the topic. Kafka ensures that each partition is read by only one consumer within the group, effectively balancing the load. For example:
- If there are 6 partitions in the topic and 3 consumers in the group, each consumer will typically handle 2 partitions.
- If a consumer leaves or joins the group, Kafka triggers a rebalance to redistribute the partitions among the remaining or additional consumers.

Of course, if you want fine-grained control, you can implement your own partition assignment strategy, but it’s not necessary for most cases, as Kafka's default handling is robust.

Would you like to explore more about how rebalancing works or custom partitioning strategies?
~~~

~~~
To implement custom partition assignment in Kafka, you can create a custom partition assignor by extending Kafka's `PartitionAssignor` interface. This allows you to define your own logic for assigning partitions to consumers within a consumer group. Here's a high-level overview of the process:

1. **Create a Custom Assignor Class**:
   - Implement the `PartitionAssignor` interface.
   - Override methods like `assign` and `subscription` to define your partition assignment logic.

2. **Configure Your Kafka Consumer**:
   - Set the `partition.assignment.strategy` property in your consumer configuration to point to your custom assignor class.

3. **Deploy and Test**:
   - Deploy your custom assignor and test it with your Kafka setup to ensure partitions are assigned as expected.

For a detailed guide and code examples, you can explore resources like [this blog](https://blog.scaledcode.com/blog/building-custom-kafka-partitioner/) or [this tutorial](https://dzone.com/articles/custom-partitioner-in-kafka-lets-take-quick-tour). Let me know if you'd like help with specific implementation steps!
~~~

~~~
Absolutely, Renan! Consumer groups are deeply connected to offsets in Kafka, and this relationship is key to ensuring proper data processing.

Offsets represent the position of a consumer in a Kafka topic partition, tracking the last record read. In the context of consumer groups:
1. **Offset Management**: Kafka maintains offsets at the consumer group level. Each consumer group has its own set of offsets, so different groups can consume data independently from the same topic without interfering with each other.
2. **Fault Tolerance**: If a consumer in the group fails or rejoins, Kafka uses the stored offsets for the group to resume processing exactly where it left off, avoiding data loss or duplication.
3. **Coordination via Commit**: Consumers in the group commit their offsets periodically (manually or automatically), ensuring the group’s progress is saved reliably. This enables consistent and efficient processing of data streams.

Would you like to dive deeper into how to handle offsets manually or configure automatic offset commits? Or perhaps you'd like to explore how offsets work during consumer group rebalancing? Let me know!
~~~

---

Serialization:
* Is used in Key and Value
* Common serializers: String (JSON), Avro, Photobuf

## Kafka and Java

### POM dependency

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.kafka</groupId>
        <artifactId>kafka_2.13</artifactId>
        <version>3.4.0</version>
    </dependency>
</dependencies>
```

### Producer

```Java
package com.learning.kafkagettingstarted.chapter6;

import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerConfig;
import org.apache.kafka.clients.producer.ProducerRecord;

import java.util.Properties;
import java.util.Random;

public class KafkaUseCaseProducer {

    public static void main(String[] args) {

        //Setup Properties for Kafka Producer
        Properties kafkaProps = new Properties();

        //List of brokers to connect to
        kafkaProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,
                "localhost:9092");

        //Serializer class used to convert Keys to Byte Arrays
        kafkaProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG,
                "org.apache.kafka.common.serialization.StringSerializer");

        //Serializer class used to convert Messages to Byte Arrays
        kafkaProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG,
                "org.apache.kafka.common.serialization.StringSerializer");

        //Create a Kafka producer from configuration
        KafkaProducer simpleProducer = new KafkaProducer(kafkaProps);

        //Publish 10 messages at 2 second intervals, with a random key
        try{

            int startKey = (new Random()).nextInt(1000) ;

            for( int i=startKey; i < startKey + 10; i++) {

                //Create a producer Record
                ProducerRecord<String,String> kafkaRecord =
                        new ProducerRecord<String,String>(
                                "kafka.usecase.students",    //Topic name
                                String.valueOf(i),          //Key for the message
                                "This is student : " + i         //Message Content
                        );

                System.out.println("Sending Message : "+ kafkaRecord.toString());

                //Publish to Kafka
                simpleProducer.send(kafkaRecord);

                Thread.sleep(2000);
            }
        }
        catch(Exception e) {

        }
        finally {
            simpleProducer.close();
        }

    }
}

```

### Consumer

```Java
package com.learning.kafkagettingstarted.chapter6;

import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;

import java.time.Duration;
import java.util.Arrays;
import java.util.Properties;

public class KafkaUseCaseConsumer {
    public static void main(String[] args) {

        //Setup Properties for consumer
        Properties kafkaProps = new Properties();

        //List of Kafka brokers to connect to
        kafkaProps.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG,
                "localhost:9092");

        //Deserializer class to convert Keys from Byte Array to String
        kafkaProps.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG,
                "org.apache.kafka.common.serialization.StringDeserializer");

        //Deserializer class to convert Messages from Byte Array to String
        kafkaProps.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG,
                "org.apache.kafka.common.serialization.StringDeserializer");

        //Consumer Group ID for this consumer
        kafkaProps.put(ConsumerConfig.GROUP_ID_CONFIG,
                "kafka-java-consumer");

        //Set to consume from the earliest message, on start when no offset is
        //available in Kafka
        kafkaProps.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG,
                "earliest");

        //Create a Consumer
        KafkaConsumer<String, String> simpleConsumer =
                new KafkaConsumer<String,String>(kafkaProps);

        //Subscribe to the kafka.learning.orders topic
        simpleConsumer.subscribe(Arrays.asList("kafka.usecase.students"));

        //Continuously poll for new messages
        while(true) {

            //Poll with timeout of 100 milli seconds
            ConsumerRecords<String, String> messages =
                    simpleConsumer.poll(Duration.ofMillis(100));

            //Print batch of records consumed
            for (ConsumerRecord<String, String> message : messages)
                System.out.println("Message fetched : " + message);
        }



    }
}

```



## **Topic Durability in Kafka**
Topic durability ensures that messages sent to a Kafka topic are not lost, even in the event of failures. This is achieved through **replication** and **acknowledgment settings**:

1. **Replication**:
   - Each topic in Kafka is divided into partitions, and each partition has multiple replicas distributed across brokers.
   - The **replication factor** determines the number of replicas for each partition. For example, a replication factor of 3 means there are 3 copies of the data.

2. **Acknowledgment Settings**:
   - Producers can configure the `acks` parameter to control durability:
     - `acks=0`: No acknowledgment from the broker; fastest but least durable.
     - `acks=1`: Acknowledgment from the leader replica only.
     - `acks=all`: Acknowledgment from all in-sync replicas (ISR), ensuring the highest durability.

3. **Configuration Parameters**:
   - `min.insync.replicas`: Specifies the minimum number of replicas that must acknowledge a write for it to be considered successful. For example, if set to 2, at least 2 replicas must be in sync.
   - `log.retention.ms` and `log.retention.bytes`: Control how long or how much data is retained in a topic.
   - `cleanup.policy`: Determines whether old data is deleted (`delete`) or compacted (`compact`).

---

## **ZooKeeper vs. KRaft (Kafka Raft)**

#### **ZooKeeper**:
- **Role**: ZooKeeper is a separate service used for managing metadata, configurations, and coordination among Kafka brokers.
- **Key Features**:
  - Leader election for brokers.
  - Metadata storage for topics, partitions, and offsets.
  - Tracks broker availability.
- **Configuration**:
  - Requires a separate ZooKeeper cluster.
  - `zookeeper.connect`: Specifies the ZooKeeper connection string in Kafka's configuration.

#### **KRaft (Kafka Raft)**:
- **Role**: Introduced in Kafka 2.8 and enhanced in Kafka 3.0, KRaft eliminates the dependency on ZooKeeper by integrating metadata management directly into Kafka.
- **Key Features**:
  - Uses the **Raft consensus algorithm** for metadata consistency.
  - Simplifies Kafka's architecture by consolidating metadata management.
  - Improves scalability and reduces operational complexity.
- **Configuration**:
  - `process.roles`: Defines the role of the Kafka process (e.g., broker, controller).
  - `controller.quorum.voters`: Specifies the quorum of controllers for metadata management.
  - `metadata.log.dir`: Directory for storing metadata logs.

#### **Architectural Diagram**:
In KRaft mode, the architecture is streamlined:
- Brokers handle both data and metadata operations.
- Controllers (using Raft) manage metadata consistency across the cluster.

For a visual representation, you can explore diagrams like [this one](https://developer.confluent.io/learn-kafka/architecture/control-plane/) or check out tutorials on setting up Kafka in KRaft mode.

---


## Kafka on Linux

[Guide](https://learn.conduktor.io/kafka/how-to-install-apache-kafka-on-linux/)

- Install JDK 11
- [Download Kafka](https://kafka.apache.org/downloads) 
- Extract in Linux
- Start Zookeeper
- Start Kafka
- Setup $Path

## Kafka on Windows

[Guide](https://learn.conduktor.io/kafka/how-to-install-apache-kafka-on-windows/)

- Install WSL2
- Install Java JDK version 11
- [Download Kafka](https://kafka.apache.org/downloads) 
- Extract in WSL2
- Start Zookeeper in WSL2
- Start Kafka in WSL2
- Setup $Path in WSL2


## Kafka on Docker

TODO

## Kafka Sample Tests

### 1. Criar o Broker
Certifique-se de que o arquivo `server.properties` está devidamente configurado no diretório do Kafka para criar um Broker. Você pode iniciar um broker com o seguinte comando:

```bash
bin/kafka-server-start.sh config/server.properties
```

### 2. Criar Tópicos com 10 Partições Cada
Depois de configurar e iniciar o broker, você pode criar dois tópicos com o comando abaixo:

**Criar o primeiro tópico:**
```bash
bin/kafka-topics.sh --create --topic my-first-topic --bootstrap-server localhost:9092 --partitions 10 --replication-factor 1
```

**Criar o segundo tópico:**
```bash
bin/kafka-topics.sh --create --topic my-second-topic --bootstrap-server localhost:9092 --partitions 10 --replication-factor 1
```

### 3. Verificar os Tópicos Criados
Para checar se os tópicos foram criados com sucesso, use este comando:
```bash
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

---

## Kafka CLI

Kafka

### Kafka Topic Management

---

## Kafka SDK

https://learn.conduktor.io/kafka/kafka-sdk-list/

https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/what-is-corretto-11.html

### Kafka with Java

https://learn.conduktor.io/kafka/creating-a-kafka-java-project-using-gradle-build-gradle/

https://learn.conduktor.io/kafka/creating-a-kafka-java-project-using-maven-pom-xml/
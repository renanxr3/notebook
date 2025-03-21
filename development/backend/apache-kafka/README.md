
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


## Kafka on Docker


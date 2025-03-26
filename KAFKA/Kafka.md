### 1. **Introduction to Kafka**
Apache Kafka is an open-source distributed event streaming platform used for building real-time data pipelines and streaming applications. It is designed to handle high-throughput, fault-tolerant, and scalable messaging.

### 2. **Coding Guidelines**
Kafka has specific coding guidelines to ensure consistency and best practices:
- Avoid cryptic abbreviations and use clear, informative variable names.
- Use `val` and `private` in Scala whenever possible.
- Follow camel case for method and variable names.
- Avoid commented-out code and unnecessary `println` statements.

### 3. **Configuration**
Kafka's configuration involves setting up brokers, topics, producers, and consumers:
- **Broker Configuration**: Includes properties like `log.dirs`, `zookeeper.connect`, and `num.partitions`.
- **Producer Configuration**: Key properties include `bootstrap.servers`, `key.serializer`, and `value.serializer`.
- **Consumer Configuration**: Includes `group.id`, `bootstrap.servers`, and `key.deserializer`.

### 4. **Producer and Consumer Programs**
Kafka producers and consumers are essential for sending and receiving messages:
- **Producer**: Sends records (key-value pairs) to Kafka topics. Example:
  ```java
  Properties props = new Properties();
  props.put("bootstrap.servers", "localhost:9092");
  props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
  props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
  Producer<String, String> producer = new KafkaProducer<>(props);
  producer.send(new ProducerRecord<>("topic", "key", "value"));
  producer.close();
  ```
- **Consumer**: Reads records from Kafka topics. 
Example:
```java
  Properties props = new Properties();
  props.put("bootstrap.servers", "localhost:9092");
  props.put("group.id", "test-group");
  props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
  props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
  KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
  consumer.subscribe(Collections.singletonList("topic"));
  while (true) {
      ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
      for (ConsumerRecord<String, String> record : records) {
          System.out.printf("offset = %d, key = %s, value = %s%n", record.offset(), record.key(), record.value());
      }
  }
```

### 5. **Real-World Examples**
Kafka is used in various industries for:
- **Fraud Detection**: Real-time monitoring of transactions.
- **Predictive Maintenance**: Analyzing machine data to predict failures.
- **Streaming Analytics**: Processing data streams for insights.

### 6. **Advanced Topics**
- **Partitioning**: Ensures data distribution across brokers.
- **Replication**: Provides fault tolerance by replicating data across brokers.
- **Stream Processing**: Kafka Streams API allows real-time data processing.
---
**Kafka for Spring Boot applications**

---

### 1. **Setting up Kafka in a Spring Boot Project**

#### Dependencies:
Add the following Kafka dependencies to your `pom.xml` file:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

---

### 2. **Kafka Configuration in Spring Boot**

#### Kafka Properties in `application.properties`:
```properties
# Kafka Broker Address
spring.kafka.bootstrap-servers=localhost:9092

# Producer Properties
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer

# Consumer Properties
spring.kafka.consumer.group-id=my-group
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.auto-offset-reset=earliest
```

#### Java-based Configuration:
You can also define a custom configuration for the producer and consumer:
```java
@Configuration
public class KafkaConfig {

    @Bean
    public ProducerFactory<String, String> producerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        configProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        configProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        return new DefaultKafkaProducerFactory<>(configProps);
    }

    @Bean
    public KafkaTemplate<String, String> kafkaTemplate() {
        return new KafkaTemplate<>(producerFactory());
    }

    @Bean
    public ConsumerFactory<String, String> consumerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "my-group");
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        return new DefaultKafkaConsumerFactory<>(props);
    }
}
```

---

### 3. **Producer Example in Spring Boot**

Create a producer service to send messages to a Kafka topic:
```java
@Service
public class KafkaProducer {

    private final KafkaTemplate<String, String> kafkaTemplate;

    public KafkaProducer(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendMessage(String topic, String message) {
        kafkaTemplate.send(topic, message);
    }
}
```

Usage:
```java
@RestController
@RequestMapping("/kafka")
public class KafkaController {

    private final KafkaProducer kafkaProducer;

    public KafkaController(KafkaProducer kafkaProducer) {
        this.kafkaProducer = kafkaProducer;
    }

    @GetMapping("/send")
    public String sendMessage(@RequestParam String message) {
        kafkaProducer.sendMessage("my-topic", message);
        return "Message sent!";
    }
}
```

---

### 4. **Consumer Example in Spring Boot**

Create a listener to consume messages:
```java
@Service
public class KafkaConsumer {

    @KafkaListener(topics = "my-topic", groupId = "my-group")
    public void listen(String message) {
        System.out.println("Received Message: " + message);
    }
}
```

---

### 5. **Real-World Scenarios**

#### a. **Log Aggregation:**
- Producers send application logs to Kafka.
- Consumers process logs in real time for monitoring and alerting.

#### b. **E-commerce Event Streaming:**
- Kafka handles events like order creation, payment processing, and inventory updates.
- Producers (microservices) send these events to specific Kafka topics.
- Consumers perform tasks like updating databases or notifying users.

#### c. **Data Pipeline for Analytics:**
- Kafka streams raw data from producers (IoT devices, logs) to analytics systems like Apache Spark.

---

### 6. **Advanced Features in Spring Kafka**

#### a. **Retry and Error Handling:**
Spring Kafka provides out-of-the-box retry functionality:
```java
@KafkaListener(topics = "my-topic", errorHandler = "myErrorHandler")
public void listen(String message) {
    // Process message
}

@Bean
public SeekToCurrentErrorHandler myErrorHandler() {
    return new SeekToCurrentErrorHandler();
}
```

#### b. **Batch Consumption:**
Consume messages in batches for better performance:
```java
@KafkaListener(topics = "my-topic", containerFactory = "batchFactory")
public void listen(List<String> messages) {
    messages.forEach(System.out::println);
}
```

---

### Configuration-Specific Challenges in Kafka

Here are some common challenges faced during Kafka configuration, along with examples and solutions:

#### 1. **Data Loss and Inconsistency**
- **Challenge**: Ensuring data consistency and preventing data loss during replication.
- **Example**: If a broker fails, replicas may fall out of sync, leading to data loss.
- **Solution**: Set an appropriate replication factor (e.g., 3) and use ISR (In-Sync Replicas) to ensure only in-sync replicas are considered for reads and writes. Monitor replication lag using tools like Kafka Manager.

#### 2. **Consumer Lag**
- **Challenge**: Consumers may lag behind producers, especially during high-throughput scenarios.
- **Example**: A consumer group processing real-time stock market data falls behind, causing delayed analytics.
- **Solution**: Optimize consumer configurations like `fetch.min.bytes` and `max.poll.records`. Scale consumers horizontally by adding more instances to the consumer group.

#### 3. **Partitioning Strategy**
- **Challenge**: Uneven distribution of data across partitions can lead to performance bottlenecks.
- **Example**: A topic with 10 partitions has most of the data concentrated in one partition, causing a hotspot.
- **Solution**: Design partitioning strategies based on data volume and access patterns. Use custom partitioners to distribute data evenly.

#### 4. **Capacity Planning**
- **Challenge**: Scaling Kafka clusters to handle increasing data volume.
- **Example**: A Kafka cluster handling IoT data experiences high latency due to insufficient brokers.
- **Solution**: Monitor metrics like throughput and storage utilization. Scale horizontally by adding brokers and partitions.

#### 5. **ZooKeeper Dependency**
- **Challenge**: Kafka relies on ZooKeeper for cluster management, which can become a single point of failure.
- **Example**: ZooKeeper downtime causes Kafka brokers to lose coordination.
- **Solution**: Use ZooKeeper ensembles with multiple nodes and enable leader election for fault tolerance.

---

**Multiple Kafka consumers**, their configuration, and examples. Multiple consumers allow parallel processing of messages from a topic, which is essential for scalability and performance in distributed systems. It'll cover key concepts, configurations, and provide Spring Boot-based code snippets.

---

### 1. **Understanding Multiple Kafka Consumers**
- **Consumer Groups**: Kafka uses consumer groups to allow multiple consumers to divide up message processing across partitions. Each partition is assigned to only one consumer within a group, ensuring no duplication.
- **Parallel Processing**: With multiple consumers in a group, messages in a topic are processed in parallel.

---

### 2. **Configuration for Multiple Consumers**

#### **Key Configurations for Consumer Groups**
Here are the configurations you need to enable multiple Kafka consumers:

```properties
# Consumer group identifier (same group for all consumers in a group)
spring.kafka.consumer.group-id=my-consumer-group

# Kafka broker address
spring.kafka.bootstrap-servers=localhost:9092

# Deserializer classes for key and value
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer

# Auto offset reset behavior
spring.kafka.consumer.auto-offset-reset=earliest
```

If you're using multiple consumer instances, they should all share the same group ID to form a consumer group.

---

### 3. **Code Example: Multiple Kafka Consumers in Spring Boot**

#### **Consumer A**
```java
@Service
public class KafkaConsumerA {

    @KafkaListener(topics = "my-topic", groupId = "my-consumer-group")
    public void listenA(String message) {
        System.out.println("Consumer A received: " + message);
    }
}
```

#### **Consumer B**
```java
@Service
public class KafkaConsumerB {

    @KafkaListener(topics = "my-topic", groupId = "my-consumer-group")
    public void listenB(String message) {
        System.out.println("Consumer B received: " + message);
    }
}
```

When multiple consumers belong to the same group (e.g., `my-consumer-group`), Kafka distributes the partitions among them. For example, if the topic has 4 partitions and 2 consumers, each consumer processes 2 partitions.

---

### 4. **Handling Partition Assignment**
You can define custom logic for partition assignment by implementing `ConsumerRebalanceListener`:
```java
@Service
public class KafkaConsumerWithRebalanceListener {

    @KafkaListener(topics = "my-topic", groupId = "my-consumer-group")
    public void listenWithRebalanceListener(String message) {
        System.out.println("Message received: " + message);
    }

    @Bean
    public ConcurrentKafkaListenerContainerFactory<String, String> kafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, String> factory = new ConcurrentKafkaListenerContainerFactory<>();
        factory.getContainerProperties().setConsumerRebalanceListener(new ConsumerRebalanceListener() {
            @Override
            public void onPartitionsRevoked(Collection<TopicPartition> partitions) {
                System.out.println("Partitions revoked: " + partitions);
            }

            @Override
            public void onPartitionsAssigned(Collection<TopicPartition> partitions) {
                System.out.println("Partitions assigned: " + partitions);
            }
        });
        return factory;
    }
}
```

---

### 5. **Real-World Example: E-commerce Application**
Imagine you’re building an e-commerce system where multiple consumers handle different types of events:
- **OrderConsumer** processes orders (topic: `order-topic`).
- **PaymentConsumer** processes payments (topic: `payment-topic`).

Here’s how you set it up:

#### **Order Consumer**
```java
@Service
public class OrderConsumer {

    @KafkaListener(topics = "order-topic", groupId = "order-group")
    public void processOrder(String order) {
        System.out.println("Processing order: " + order);
    }
}
```

#### **Payment Consumer**
```java
@Service
public class PaymentConsumer {

    @KafkaListener(topics = "payment-topic", groupId = "payment-group")
    public void processPayment(String payment) {
        System.out.println("Processing payment: " + payment);
    }
}
```

---

### 6. **Scaling Multiple Consumers**
- **Horizontal Scaling**: Add more consumer instances to the group to process messages in parallel.
- **Partition Configuration**: Increase the number of partitions in the topic to ensure effective distribution of messages across consumers.

#### Example:
If you have a topic with 6 partitions:
- **3 Consumers in the same group**: Each consumer processes 2 partitions.
- **Adding a 4th Consumer**: Some consumers will handle multiple partitions, as partitions > consumers.

---

If you want to share the **same message** with multiple consumers or subscribers, you can use Kafka's **publish-subscribe** model effectively. In Kafka, a topic can have multiple consumer groups, and each group acts as a separate set of subscribers. Here’s how it works and how you can implement it:

---

### 1. **How Publish-Subscribe Works in Kafka**
- **Multiple Consumer Groups**: Kafka allows you to create multiple consumer groups for a single topic. Each group receives a copy of the messages from the topic.
- **Use Case**: Imagine a notification system where you want to send the same event (e.g., "Order Placed") to:
  - Logging service (Consumer Group A)
  - Analytics service (Consumer Group B)
  - Notification service (Consumer Group C)

In this scenario, each consumer group processes its own copy of the message independently.

---

### 2. **Configuration for Multiple Consumer Groups**
Define separate group IDs in your `application.properties` for each consumer group:
```properties
# Consumer Group A
spring.kafka.consumer.group-id=consumer-group-a

# Consumer Group B
spring.kafka.consumer.group-id=consumer-group-b

# Consumer Group C
spring.kafka.consumer.group-id=consumer-group-c

spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.auto-offset-reset=earliest
```

---

### 3. **Code Example for Sharing Messages**

#### **Producer Service**
The producer sends messages to a topic:
```java
@Service
public class KafkaProducer {

    private final KafkaTemplate<String, String> kafkaTemplate;

    public KafkaProducer(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendMessage(String topic, String message) {
        kafkaTemplate.send(topic, message);
    }
}
```

#### **Multiple Consumers**

##### **Consumer for Logging Service**
```java
@Service
public class LoggingConsumer {

    @KafkaListener(topics = "shared-topic", groupId = "consumer-group-a")
    public void listenLogging(String message) {
        System.out.println("Logging Consumer received: " + message);
    }
}
```

##### **Consumer for Analytics Service**
```java
@Service
public class AnalyticsConsumer {

    @KafkaListener(topics = "shared-topic", groupId = "consumer-group-b")
    public void listenAnalytics(String message) {
        System.out.println("Analytics Consumer received: " + message);
    }
}
```

##### **Consumer for Notification Service**
```java
@Service
public class NotificationConsumer {

    @KafkaListener(topics = "shared-topic", groupId = "consumer-group-c")
    public void listenNotification(String message) {
        System.out.println("Notification Consumer received: " + message);
    }
}
```

In this setup:
- The producer sends a message to `shared-topic`.
- Each consumer group (`consumer-group-a`, `consumer-group-b`, `consumer-group-c`) independently consumes the message.

---

### 4. **Real-World Use Case Example**
Imagine you’re running a **multi-service application**:
- When a user places an order:
  - **Consumer Group A (Logging)** stores the event in application logs.
  - **Consumer Group B (Analytics)** updates statistical dashboards.
  - **Consumer Group C (Notification)** sends an email or SMS notification to the user.

Each service operates independently because they belong to separate consumer groups.

---

### 5. **Scaling and Best Practices**
#### Scaling:
- Add more partitions to the topic if the consumer workload increases.
- Add more consumers within the same group to handle high-throughput data.

#### Best Practices:
- Ensure group IDs are unique for truly independent consumption of messages.
- Use monitoring tools like Kafka Manager to track consumer group performance.
- Avoid excessive consumer creation; use logical grouping based on service requirements.

---

how Kafka handles message consumption and delivery to multiple consumers?

### 1. **Messages Are Not Deleted After Being Consumed**
In Kafka, messages are not deleted immediately after being consumed. Instead, Kafka uses a **log-based storage system** where messages are retained for a configured period, regardless of whether consumers have read them.

#### **Retention Policy**
- Messages in Kafka are retained based on the topic's configuration. For example:
  ```properties
  log.retention.hours=168
  log.retention.bytes=1073741824
  ```
  - `log.retention.hours`: Defines how long messages are stored (e.g., 7 days).
  - `log.retention.bytes`: Defines the maximum size of the log before older messages are deleted.

So, even if one consumer has read a message, other consumers can still access it as long as it is retained in the log.

---

### 2. **How Kafka Shares Messages with Multiple Consumers**

#### **Consumer Groups and Partition Assignment**
Kafka ensures that within a single **consumer group**, each partition is assigned to one consumer, meaning messages are consumed only once per group. However, if you use **different consumer groups**, each group acts as an independent subscriber and receives the same set of messages.

#### **Example: Multiple Subscriber Groups**
Consider a topic `shared-topic` with 6 partitions:
- **Group A** (logging consumers) receives all messages independently.
- **Group B** (analytics consumers) also receives the same messages, independent of Group A.

This happens because Kafka tracks the offset (position of the message in the log) separately for each consumer group.

---

### 3. **How Kafka Tracks Message Consumption**

Kafka uses **consumer offsets** to keep track of which messages a consumer group has read. The offset is managed per partition:
- Offset is committed by the consumer after processing a message (manually or automatically).
- If a consumer restarts or crashes, Kafka uses the last committed offset to resume reading.

Since the offset is tracked **by consumer group**, different groups can process the same message independently.

---

### 4. **Code Example: Sharing Messages with Multiple Consumer Groups**

Here’s an example setup:

#### **Producer Service**
```java
@Service
public class KafkaProducer {

    private final KafkaTemplate<String, String> kafkaTemplate;

    public KafkaProducer(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendMessage(String topic, String message) {
        kafkaTemplate.send(topic, message);
    }
}
```

#### **Logging Consumer Group (Group A)**
```java
@Service
public class LoggingConsumer {

    @KafkaListener(topics = "shared-topic", groupId = "logging-group")
    public void listenLogging(String message) {
        System.out.println("Logging Consumer received: " + message);
    }
}
```

#### **Analytics Consumer Group (Group B)**
```java
@Service
public class AnalyticsConsumer {

    @KafkaListener(topics = "shared-topic", groupId = "analytics-group")
    public void listenAnalytics(String message) {
        System.out.println("Analytics Consumer received: " + message);
    }
}
```

Each group processes messages independently, ensuring all consumers in different groups receive the same messages.

---

### 5. **Retention Configuration for Message Sharing**

If your use case requires that **all messages are available to all consumer groups**, ensure the retention policy is appropriately configured:
- Retain messages for a longer duration using `log.retention.hours`.
- Configure sufficient storage with `log.retention.bytes`.

---

### 6. **Alternative Approaches**
If real-time sharing to multiple consumers is critical, consider:
- **Kafka Streams**: Process messages in real-time across multiple applications.
- **Replication**: Use multiple topics or partitions for message duplication to different services.

---

In Kafka, you actually don't need to wait for all subscribers (or consumer groups) to read a message before it is deleted. Kafka's design ensures messages are retained based on a **retention policy**, not the consumption status by subscribers. Let me clarify:

---

### 1. **Message Retention is Independent of Consumption**
Kafka keeps messages in the topic for a preconfigured time or size limit:
- **Retention by Time**: Messages are stored for a defined period (e.g., 7 days).
- **Retention by Size**: Messages are retained until the topic log reaches a certain size.

Even after a consumer (or group) reads the message, it remains in the log until it expires based on the retention settings. This means new consumers or groups can still read those messages during the retention period.

---

### 2. **Offset Management**
Each consumer group tracks its own **offsets**, which indicate the last-read message in each partition. Kafka stores these offsets either:
- In ZooKeeper (legacy method).
- In a special internal topic called `__consumer_offsets` (modern method).

Because offsets are managed per group, different groups can process the same messages independently, without interference.

---

### 3. **How Retention Works for Multiple Subscribers**
Here’s how Kafka retains messages for multiple consumer groups:
1. **Producer** sends a message to a topic.
2. Kafka retains the message in the log for the configured retention period (e.g., 7 days).
3. Each consumer group reads the message independently and commits its offset.
4. The message is only deleted after the retention period expires, regardless of whether it has been read by all consumer groups.

---

### 4. **Example Scenario**
Let’s say:
- **Topic**: `orders-topic`
- **Consumer Group A**: Processes orders for analytics.
- **Consumer Group B**: Sends order notifications to customers.
- **Retention Policy**: 3 days.

1. **Producer** sends an order message to `orders-topic`.
2. Consumer Group A reads and processes the message immediately.
3. Consumer Group B reads the message later due to some delay or downtime.
4. The message remains in the topic for 3 days, ensuring both groups can read it.

---

### 5. **Why Kafka Doesn't Wait for Subscribers**
Kafka's design avoids the inefficiency of waiting for all consumers to read a message before deleting it. Instead:
- Messages remain in the log for a fixed period or size.
- Each consumer group operates independently using offsets.

This approach enables high scalability and flexibility, as Kafka doesn’t need to track individual subscribers’ consumption status for message deletion.

---

### 6. **Adjusting Retention Based on Use Case**
If you need a longer period for all subscribers to consume messages:
- Increase the retention time in the topic configuration:
  ```bash
  bin/kafka-topics.sh --zookeeper localhost:2181 --alter --topic orders-topic --config retention.ms=604800000
  ```
  (This sets retention to 7 days.)

# 50 вопросов и ответов по Apache Kafka и Kafka Streams для Java-собеседований

## Содержание

### Основы Kafka (вопросы 1-20)

1. [Что такое Apache Kafka?](#вопрос-1)
2. [Для чего используется Kafka?](#вопрос-2)
3. [Как устроена архитектура Kafka?](#вопрос-3)
4. [Что такое продюсер в Kafka?](#вопрос-4)
5. [Что такое консюмер в Kafka?](#вопрос-5)
6. [Что такое Topic в Kafka?](#вопрос-6)
7. [Что такое Partition и зачем он нужен?](#вопрос-7)
8. [Что такое Offset?](#вопрос-8)
9. [Что такое брокер в Kafka?](#вопрос-9)
10. [Что делает ZooKeeper в Kafka?](#вопрос-10)
11. [Как устроен Consumer Group?](#вопрос-11)
12. [В чем функциональная разница между Key и Value в сообщениях Kafka?](#вопрос-12)
13. [Что такое Replication в Kafka?](#вопрос-13)
14. [Что такое ISR (In-Sync Replica)?](#вопрос-14)
15. [Что такое лидер партиции?](#вопрос-15)
16. [Как происходит запись и чтение сообщений в Kafka?](#вопрос-16)
17. [В чем разница между "at least once" и "exactly once" delivery?](#вопрос-17)
18. [Какая модель доставки поддерживается по умолчанию?](#вопрос-18)
19. [Как гарантировать доставку exactly-once в Kafka?](#вопрос-19)
20. [Что такое retention policy в Kafka?](#вопрос-20)

### Конфигурация и практика (вопросы 21-35)

21. [Что такое компакция логов (log compaction)?](#вопрос-21)
22. [Какие форматы сериализации поддерживаются в Kafka?](#вопрос-22)
23. [Что такое Avro и как его использовать с Kafka?](#вопрос-23)
24. [Для чего нужен Schema Registry?](#вопрос-24)
25. [Что такое Producer Acknowledgement?](#вопрос-25)
26. [В чем разница между "acks=0", "acks=1" и "acks=all"?](#вопрос-26)
27. [Что такое delivery.timeout.ms и linger.ms?](#вопрос-27)
28. [Как работает Consumer Poll?](#вопрос-28)
29. [Что такое Consumer Offset Commit?](#вопрос-29)
30. [Какие существуют стратегии балансировки для consumer group?](#вопрос-30)
31. [Как происходит ребалансировка потребителей?](#вопрос-31)
32. [Что такое dead letter queue в Kafka?](#вопрос-32)
33. [Как создать Producer в Java?](#вопрос-33)
34. [Как создать Consumer в Java?](#вопрос-34)
35. [Как настроить Kafka в Spring Boot?](#вопрос-35)

### Kafka Streams (вопросы 36-50)

36. [Что такое Kafka Streams?](#вопрос-36)
37. [Какая основная задача Kafka Streams API?](#вопрос-37)
38. [Что такое KStream и KTable?](#вопрос-38)
39. [Как реализуется join в Kafka Streams?](#вопрос-39)
40. [Что такое windowing в Kafka Streams?](#вопрос-40)
41. [Как происходит управление состоянием (state) в Streams?](#вопрос-41)
42. [Для чего используются state stores?](#вопрос-42)
43. [Что такое punctuate в Kafka Streams?](#вопрос-43)
44. [В чем разница между .map(), .flatMap(), .filter() в Streams?](#вопрос-44)
45. [Как обработать ошибку в процессоре Streams?](#вопрос-45)
46. [Что такое topology в Kafka Streams?](#вопрос-46)
47. [Как создать простое Kafka Streams приложение?](#вопрос-47)
48. [Как выполнять агрегирование в Streams?](#вопрос-48)
49. [Что такое interactive queries в Kafka Streams?](#вопрос-49)
50. [Какие best practices для продакшен использования Kafka?](#вопрос-50)

***

## Вопрос 1

**Что такое Apache Kafka?**

**Ответ:** Apache Kafka — это распределённая платформа потоковой передачи сообщений (streaming), предназначенная для высокого пропускного потока, масштабируемости и отказоустойчивости. В основе Kafka — лог событий, организованный по темам (topics), который хранит и передает сообщения между продюсерами и консюмерами.

***

## Вопрос 2

**Для чего используется Kafka?**

**Ответ:** Kafka применяется для построения event-driven архитектур, передачи логов, потоковой обработки данных, интеграции микросервисов, обработки кликов, телеметрии, мониторинга и других сценариев, где требуется быстрая, масштабируемая и надежная обмен данными.

***

## Вопрос 3

**Как устроена архитектура Kafka?**

**Ответ:** В архитектуре Kafka основные компоненты — брокеры (серверы), ZooKeeper для координации, топики с партициями, продюсеры, консюмеры и consumer группы. Messages хранятся и читаются по партициям, обеспечивается высокая доступность посредством репликации. ZooKeeper управляет распределением и отказоустойчивостью брокеров.

***

## Вопрос 4

**Что такое продюсер в Kafka?**

**Ответ:** Продюсер (Producer) — это клиент Kafka, который публикует сообщения в указанный топик. Он сам выбирает ключ сообщения, может управлять подтверждениями доставки (acks) и сериализует данные в байтовую строку.

***

## Вопрос 5

**Что такое консюмер в Kafka?**

**Ответ:** Консюмер (Consumer) — это приложение, которое подписывается на один или более топиков (обычно через consumer group) и получает сообщения, держа у себя позицию чтения (offset) на каждой партиции.

***

## Вопрос 6

**Что такое Topic в Kafka?**

**Ответ:** Topic — это логическая сущность, по сути "категория" для сообщений, куда продюсер отправляет и откуда консюмер читает сообщения. Топик может иметь несколько партиций для масштабирования.

---

## Вопрос 7

**Что такое Partition и зачем он нужен?**

**Ответ:** Partition — это физическая сегментация топика на независимые части сообщения. Масштабирование достигается благодаря параллельной записи/чтению, а отказоустойчивость — через репликацию. На одну партицию всегда назначен лидер.

***

## Вопрос 8

**Что такое Offset?**

**Ответ:** Offset — это уникальный номер позиции сообщения в партиции. Каждый консюмер хранит свой offset и может перемещаться по ним, читать из нужного места, а также повторно забирать сообщения.

***

## Вопрос 9

**Что такое брокер в Kafka?**

**Ответ:** Брокер — это сервер Kafka, отвечающий за хранение логов, управление топиками и партициями, доставку сообщений, репликацию и обработку запросов от клиентов.

***

## Вопрос 10

**Что делает ZooKeeper в Kafka?**

**Ответ:** ZooKeeper — система координации, используется для хранения данных о составе кластера, лидерства партиций, отслеживания состояния брокеров. С версии Kafka 2.8+ возможна работа без ZooKeeper благодаря KRaft (native consensus).

***

## Вопрос 11

**Как устроен Consumer Group?**

**Ответ:** Consumer Group — группа консюмеров, совместно читающая топик. Каждая партиция назначается ровно одному члену группы (для параллелизма); offset управление ведется на уровне группы.

***

## Вопрос 12

**В чем функциональная разница между Key и Value в сообщениях Kafka?**

**Ответ:** Key позволяет контролировать, в какую партицию попадет сообщение (partitioning), Value — это payload данных. Сообщения с одинаковым key гарантированно попадают в одну партицию.

***

## Вопрос 13

**Что такое Replication в Kafka?**

**Ответ:** Replication — дублирование данных партиции между несколькими брокерами для обеспечения отказоустойчивости и доступности.

***

## Вопрос 14

**Что такое ISR (In-Sync Replica)?**

**Ответ:** ISR — список реплик партиции, которые обновляют свои данные синхронно с лидером. Только членам ISR разрешено быть претендентом на лидерство.

---

## Вопрос 15

**Что такое лидер партиции?**

**Ответ:** Лидер — брокер, на котором активная копия партиции; все операции записи/чтения проходят через него. Реплики синхронизированы с лидером.

---

## Вопрос 16

**Как происходит запись и чтение сообщений в Kafka?**

**Ответ:** Продюсер пишет сообщение в партицию топика (обычно через leader), консюмер читает их начиная с offset. Проходит процесс сериализации/deserialization, репликации, записи в лог.

***

## Вопрос 17

**В чем разница между "at least once" и "exactly once" delivery?**

**Ответ:** "At least once" — гарантирует, что сообщение будет доставлено минимум один раз (но возможно повторно). "Exactly once" — гарантирует ровно одну доставку (без дублей), требует транзакций.

***

## Вопрос 18

**Какая модель доставки поддерживается по умолчанию?**

**Ответ:** По умолчанию — "at least once". Возможна loss, duplication при сбое повторной отправке.

***

## Вопрос 19

**Как гарантировать доставку exactly-once в Kafka?**

**Ответ:** Использовать transactional producer, idempotent producer, включить поддержку транзакций и соответствующей commit логики и настройки consumers.

***

## Вопрос 20

**Что такое retention policy в Kafka?**

**Ответ:** Retention — стратегия "жизни" сообщения; после времени хранения или достижения лимита размера partition сообщения удаляются. Настраивается через параметры topic-level.

***

## Вопрос 21

**Что такое компакция логов (log compaction)?**

**Ответ:** Log compaction — режим, когда Kafka хранит последние сообщения по одинаковым ключам, удаляя устаревшие версии, но не все. Используется для восстановления состояния (например, кэш).

***

## Вопрос 22

**Какие форматы сериализации поддерживаются в Kafka?**

**Ответ:** Популярные форматы: String, JSON, Avro, Protobuf, Thrift, а также пользовательские сериализаторы через интерфейс Serializer.

***

## Вопрос 23

**Что такое Avro и как его использовать с Kafka?**

**Ответ:** Avro — бинарный формат сериализации с поддержкой схем. С Kafka используется совместно с Confluent Schema Registry, позволяя безопасно эволюционировать формат данных.

***

## Вопрос 24

**Для чего нужен Schema Registry?**

**Ответ:** Сервис для регистрации и хранения схем сериализации (например, Avro/Protobuf); продюсер и консюмер могут валидировать сообщения относительно схемы.

***

## Вопрос 25

**Что такое Producer Acknowledgement?**

**Ответ:** Producer ждёт подтверждение приема сообщения от брокера ("acks"); позволяет балансировать между производительностью и гарантиями доставки.

---

## Вопрос 26

**В чем разница между "acks=0", "acks=1" и "acks=all"?**

**Ответ:**
- "acks=0": не ждет подтверждений, высокая скорость, нет гарантий;
- "acks=1": ждет подтверждение только от лидера;
- "acks=all": ждет подтверждение от всех ISR — высокая надежность.

***

## Вопрос 27

**Что такое delivery.timeout.ms и linger.ms?**

**Ответ:**
- delivery.timeout.ms — максимальное время ожидания отправки (fail при превышении);
- linger.ms — задержка перед отправкой батча сообщений для оптимизации throughput.

***

## Вопрос 28

**Как работает Consumer Poll?**

**Ответ:** Консюмер периодически вызывает poll(), забирая сообщения из партиций (batch polling). Без poll консюмер считается "мертвым" и исключается из группы.

***

## Вопрос 29

**Что такое Consumer Offset Commit?**

**Ответ:** Операция сохранения позиции на партиции ("offset commit"); может быть автоматическим (enable.auto.commit) или вручную (commitSync/commitAsync).

***

## Вопрос 30

**Какие существуют стратегии балансировки для consumer group?**

**Ответ:** Основные стратегии: Range, RoundRobin, Sticky, CooperativeSticky. Они определяют, как партиции распределяются между участниками группы.

---

## Вопрос 31

**Как происходит ребалансировка потребителей?**

**Ответ:** При изменении состава группы Kafka перераспределяет партиции: консюмеры "замораживаются", получают новые assignment, возобновляют poll с обновлённым списком партиций.

***

## Вопрос 32

**Что такое dead letter queue в Kafka?**

**Ответ:** DLQ — специальный топик для сообщений с ошибкой обработки (например, десериализации). Позволяет не терять ошибочные события и разбирать их отдельно.

***

## Вопрос 33

**Как создать Producer в Java?**

**Ответ:** Producer создается с помощью конфигурации Properties и KafkaProducer класса.

**Пример кода:**

```java
import org.apache.kafka.clients.producer.*;
import org.apache.kafka.common.serialization.StringSerializer;
import java.util.Properties;

public class SimpleProducer {
    public static void main(String[] args) {
        Properties props = new Properties();
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        props.put(ProducerConfig.ACKS_CONFIG, "all");
        props.put(ProducerConfig.RETRIES_CONFIG, 3);
        props.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, true);
        
        KafkaProducer<String, String> producer = new KafkaProducer<>(props);
        
        try {
            for (int i = 0; i < 100; i++) {
                ProducerRecord<String, String> record = 
                    new ProducerRecord<>("my-topic", "key-" + i, "value-" + i);
                
                producer.send(record, (metadata, exception) -> {
                    if (exception != null) {
                        exception.printStackTrace();
                    } else {
                        System.out.printf("Sent: key=%s, value=%s, partition=%d, offset=%d%n",
                            record.key(), record.value(), 
                            metadata.partition(), metadata.offset());
                    }
                });
            }
        } finally {
            producer.close();
        }
    }
}
```

***

## Вопрос 34

**Как создать Consumer в Java?**

**Ответ:** Consumer создается с помощью KafkaConsumer класса и конфигурации для подписки на топики.

**Пример кода:**

```java
import org.apache.kafka.clients.consumer.*;
import org.apache.kafka.common.serialization.StringDeserializer;
import java.time.Duration;
import java.util.Collections;
import java.util.Properties;

public class SimpleConsumer {
    public static void main(String[] args) {
        Properties props = new Properties();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "my-consumer-group");
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");
        props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false);
        
        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
        consumer.subscribe(Collections.singletonList("my-topic"));
        
        try {
            while (true) {
                ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
                
                for (ConsumerRecord<String, String> record : records) {
                    System.out.printf("Received: key=%s, value=%s, partition=%d, offset=%d%n",
                        record.key(), record.value(), 
                        record.partition(), record.offset());
                }
                
                // Manual commit after processing
                consumer.commitSync();
            }
        } finally {
            consumer.close();
        }
    }
}
```

***

## Вопрос 35

**Как настроить Kafka в Spring Boot?**

**Ответ:** В Spring Boot используется автоконфигурация через application.yml и аннотации @KafkaListener.

**Конфигурация application.yml:**

```yaml
spring:
  kafka:
    bootstrap-servers: localhost:9092
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
      acks: all
      retries: 3
      properties:
        enable.idempotence: true
        linger.ms: 5
    consumer:
      group-id: my-spring-group
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.springframework.kafka.support.serializer.JsonDeserializer
      auto-offset-reset: earliest
      enable-auto-commit: false
      properties:
        spring.json.trusted.packages: "com.example.model"
    listener:
      ack-mode: manual_immediate
```

**Java конфигурация:**

```java
@Service
public class KafkaService {
    
    @Autowired
    private KafkaTemplate<String, Object> kafkaTemplate;
    
    public void sendMessage(String topic, Object message) {
        kafkaTemplate.send(topic, message);
    }
    
    @KafkaListener(topics = "my-topic", groupId = "my-spring-group")
    public void listen(@Payload String message, 
                      @Header(KafkaHeaders.RECEIVED_TOPIC) String topic,
                      @Header(KafkaHeaders.RECEIVED_PARTITION_ID) int partition,
                      @Header(KafkaHeaders.OFFSET) long offset,
                      Acknowledgment ack) {
        System.out.printf("Received: %s from topic: %s, partition: %d, offset: %d%n",
                message, topic, partition, offset);
        
        // Manual acknowledge
        ack.acknowledge();
    }
}
```

***

## Вопрос 36

**Что такое Kafka Streams?**

**Ответ:** Это клиентская библиотека для потоковой обработки, позволяющая строить топологии: трансформации, агрегирование, объединение данных прямо в JVM-приложении (Java/Scala/Kotlin).

***

## Вопрос 37

**Какая основная задача Kafka Streams API?**

**Ответ:** Простой, эффективный и масштабируемый "stream processing" — обработка, фильтрация, агрегация, объединение, windowed операции потоков событий Kafka.

***

## Вопрос 38

**Что такое KStream и KTable?**

**Ответ:**
- KStream — "лог" событий (stream) — неизменяемый поток;
- KTable — "таблица" состояний, где по ключам хранится последнее значение, поддерживает апдейты/агрегации.

***

## Вопрос 39

**Как реализуется join в Kafka Streams?**

**Ответ:** Streams поддерживают stream-stream join, stream-table join, table-table join; windowed join позволяет "склеивать" события, пришедшие близко по времени.

***

## Вопрос 40

**Что такое windowing в Kafka Streams?**

**Ответ:** Window — временной диапазон для группировки и агрегации событий (например, "сумма за 5 минут"). Используется для расчёта метрик, обработки поступающих данных "окнами".

***

## Вопрос 41

**Как происходит управление состоянием (state) в Streams?**

**Ответ:** Локальные state stores используются для хранения промежуточных результатов, автоматически реплицируются и поддерживают отказоустойчивость через changelog topics.

***

## Вопрос 42

**Для чего используются state stores?**

**Ответ:** State store — локальное хранилище для агрегатов, временных окон, таблиц; обеспечивает быстрый доступ к состоянию и поддерживает интерактивные запросы.

***

## Вопрос 43

**Что такое punctuate в Kafka Streams?**

**Ответ:** Метод, который триггерит обработку/выгрузку агрегированных данных из Processor API по расписанию или при накоплении определенного состояния.

***

## Вопрос 44

**В чем разница между .map(), .flatMap(), .filter() в Streams?**

**Ответ:**
- map трансформирует ключ/значение в другое;
- flatMap — может вернуть несколько пар ключ/значение;
- filter — отбирает события по предикату.

***

## Вопрос 45

**Как обработать ошибку в процессоре Streams?**

**Ответ:** Реализовать custom обработчик ошибок или использовать .transform(), .catch(), стереть ошибочные сообщения в DLQ, либо обработать ошибку в stateful processor.

***

## Вопрос 46

**Что такое topology в Kafka Streams?**

**Ответ:** Topology — схема, набор связанных процессоров и хранилищ состояний, определяющая направление и этапы обработки потока событий.

***

## Вопрос 47

**Как создать простое Kafka Streams приложение?**

**Ответ:** Простое приложение WordCount показывает основы Kafka Streams API.

**Пример кода:**

```java
import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.common.serialization.Serdes;
import org.apache.kafka.streams.KafkaStreams;
import org.apache.kafka.streams.StreamsBuilder;
import org.apache.kafka.streams.StreamsConfig;
import org.apache.kafka.streams.kstream.*;

import java.util.Arrays;
import java.util.Properties;

public class WordCountApp {
    public static void main(String[] args) {
        Properties config = new Properties();
        config.put(StreamsConfig.APPLICATION_ID_CONFIG, "wordcount-application");
        config.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        config.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");
        config.put(StreamsConfig.DEFAULT_KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass());
        config.put(StreamsConfig.DEFAULT_VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass());

        StreamsBuilder builder = new StreamsBuilder();

        // Входной поток текста
        KStream<String, String> textLines = builder.stream("word-count-input");
        
        // Обработка: подсчет слов
        KTable<String, Long> wordCounts = textLines
            .mapValues(textLine -> textLine.toLowerCase())
            .flatMapValues(textLine -> Arrays.asList(textLine.split("\\s+")))
            .selectKey((key, word) -> word)
            .groupByKey()
            .count(Named.as("Counts"));

        // Вывод результата
        wordCounts.toStream().to("word-count-output", 
            Produced.with(Serdes.String(), Serdes.Long()));

        KafkaStreams streams = new KafkaStreams(builder.build(), config);
        
        // Graceful shutdown
        Runtime.getRuntime().addShutdownHook(new Thread(streams::close));
        
        streams.start();
    }
}
```

**Конфигурация Kafka Streams в Spring Boot:**

```yaml
spring:
  kafka:
    streams:
      application-id: word-count-app
      bootstrap-servers: localhost:9092
      default-key-serde: org.apache.kafka.common.serialization.Serdes$StringSerde
      default-value-serde: org.apache.kafka.common.serialization.Serdes$StringSerde
      properties:
        processing.guarantee: exactly_once
        commit.interval.ms: 1000
```

***

## Вопрос 48

**Как выполнять агрегирование в Streams?**

**Ответ:** С помощью .groupByKey().aggregate(), .count(), .reduce(), .windowedBy(); State Store хранит агрегаты по ключам или окнам.

**Пример агрегации:**

```java
KTable<String, Integer> aggregated = stream
    .groupByKey()
    .aggregate(
        () -> 0,  // initializer
        (key, value, aggregate) -> aggregate + value.length(),  // aggregator
        Materialized.<String, Integer, KeyValueStore<Bytes, byte[]>>as("aggregated-store")
            .withKeySerde(Serdes.String())
            .withValueSerde(Serdes.Integer())
    );

// Windowed aggregation
KTable<Windowed<String>, Long> windowedCounts = stream
    .groupByKey()
    .windowedBy(TimeWindows.of(Duration.ofMinutes(5)))
    .count();
```

***

## Вопрос 49

**Что такое interactive queries в Kafka Streams?**

**Ответ:** Возможность делать внешние запросы к локальным state stores приложения напрямую (REST/gRPC), без дополнительных запросов к Kafka.

**Пример Interactive Queries:**

```java
@RestController
public class QueryController {
    
    @Autowired
    private KafkaStreams kafkaStreams;
    
    @GetMapping("/count/{word}")
    public Long getWordCount(@PathVariable String word) {
        ReadOnlyKeyValueStore<String, Long> store = 
            kafkaStreams.store("word-count-store", QueryableStoreTypes.keyValueStore());
        
        return store.get(word);
    }
    
    @GetMapping("/range/{from}/{to}")  
    public Map<String, Long> getRange(@PathVariable String from, @PathVariable String to) {
        ReadOnlyKeyValueStore<String, Long> store = 
            kafkaStreams.store("word-count-store", QueryableStoreTypes.keyValueStore());
            
        Map<String, Long> result = new HashMap<>();
        try (KeyValueIterator<String, Long> range = store.range(from, to)) {
            while (range.hasNext()) {
                KeyValue<String, Long> next = range.next();
                result.put(next.key, next.value);
            }
        }
        return result;
    }
}
```

***

## Вопрос 50

**Какие best practices для продакшен использования Kafka?**

**Ответ:**

**Конфигурация брокера:**
```properties
# server.properties
num.network.threads=8
num.io.threads=16
socket.send.buffer.bytes=102400
socket.receive.buffer.bytes=102400
socket.request.max.bytes=104857600

# Replication
default.replication.factor=3
min.insync.replicas=2

# Log settings
log.retention.hours=168
log.segment.bytes=1073741824
log.cleanup.policy=delete

# JVM settings
-Xmx6g -Xms6g
-XX:+UseG1GC
-XX:MaxGCPauseMillis=20
```

**Best Practices:**
- Использовать репликацию ≥3
- Настроить monitor/alert на lag ошибок
- Хранить offsets во внешнем хранилище
- Оптимизировать размер/количество партиций
- Разделять prod/dev инфраструктуру
- Применять idempotent producer, транзакции при необходимости
- Документировать схемы данных
- Мониторить JVM метрики (GC, heap)
- Использовать SSL/SASL для безопасности
- Настроить backup/restore процедуры

**Мониторинг:**
```yaml
# Prometheus metrics
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
  metrics:
    export:
      prometheus:
        enabled: true
    distribution:
      percentiles-histogram:
        kafka: true
```
# 50 вопросов и ответов по Apache Kafka и Kafka Streams для Java-разработчиков

## Содержание

### Основы Kafka (вопросы 1-15)
1. [Что такое Apache Kafka?](#вопрос-1)
2. [Для чего используется Kafka?](#вопрос-2)
3. [Как устроена архитектура Kafka?](#вопрос-3)
4. [Что такое Topic в Kafka?](#вопрос-4)
5. [Что такое Partition и зачем он нужен?](#вопрос-5)
6. [Что такое Offset?](#вопрос-6)
7. [Что такое Producer в Kafka?](#вопрос-7)
8. [Что такое Consumer в Kafka?](#вопрос-8)
9. [Что такое Consumer Group?](#вопрос-9)
10. [Что такое Broker в Kafka?](#вопрос-10)
11. [Что делает ZooKeeper в Kafka?](#вопрос-11)
12. [В чем разница между Key и Value в сообщениях?](#вопрос-12)
13. [Что такое Replication в Kafka?](#вопрос-13)
14. [Что такое ISR (In-Sync Replica)?](#вопрос-14)
15. [Что такое Leader и Follower партиций?](#вопрос-15)

### Продюсеры и консюмеры (вопросы 16-30)
16. [Что такое Producer Acknowledgement (acks)?](#вопрос-16)
17. [В чем разница между acks=0, acks=1 и acks=all?](#вопрос-17)
18. [Что такое Idempotent Producer?](#вопрос-18)
19. [Что такое Transactional Producer?](#вопрос-19)
20. [В чем разница между at-least-once, at-most-once и exactly-once?](#вопрос-20)
21. [Что такое Consumer Offset?](#вопрос-21)
22. [Как работает Consumer Poll?](#вопрос-22)
23. [Что такое Offset Commit?](#вопрос-23)
24. [Что такое Auto Offset Reset?](#вопрос-24)
25. [Какие стратегии балансировки Consumer Group существуют?](#вопрос-25)
26. [Что такое Rebalance и когда он происходит?](#вопрос-26)
27. [Что такое delivery.timeout.ms и linger.ms?](#вопрос-27)
28. [Что такое batch.size и buffer.memory?](#вопрос-28)
29. [Что такое max.poll.records и max.poll.interval.ms?](#вопрос-29)
30. [Что такое Consumer Lag?](#вопрос-30)

### Хранение и управление данными (вопросы 31-40)
31. [Что такое Retention Policy?](#вопрос-31)
32. [Что такое Log Compaction?](#вопрос-32)
33. [Какие форматы сериализации поддерживает Kafka?](#вопрос-33)
34. [Что такое Schema Registry?](#вопрос-34)
35. [Что такое Avro и зачем он нужен?](#вопрос-35)
36. [Как обеспечить порядок сообщений в Kafka?](#вопрос-36)
37. [Что такое Dead Letter Queue (DLQ)?](#вопрос-37)
38. [Как масштабировать Kafka кластер?](#вопрос-38)
39. [Что такое Partition Reassignment?](#вопрос-39)
40. [Как мониторить Kafka кластер?](#вопрос-40)

### Kafka Streams (вопросы 41-50)
41. [Что такое Kafka Streams?](#вопрос-41)
42. [Что такое KStream и KTable?](#вопрос-42)
43. [Что такое GlobalKTable?](#вопрос-43)
44. [Как работают Join операции в Kafka Streams?](#вопрос-44)
45. [Что такое Windowing в Kafka Streams?](#вопрос-45)
46. [Что такое State Store?](#вопрос-46)
47. [Что такое Topology в Kafka Streams?](#вопрос-47)
48. [Как работает exactly-once в Kafka Streams?](#вопрос-48)
49. [Что такое Interactive Queries?](#вопрос-49)
50. [Какие Best Practices для Kafka в production?](#вопрос-50)

***

## Вопрос 1

**Что такое Apache Kafka?**

Apache Kafka — распределённая платформа потоковой передачи событий (event streaming), разработанная для высокопроизводительной публикации, хранения и обработки потоков записей в реальном времени. Kafka работает как распределённый журнал (distributed commit log), где продюсеры записывают данные в топики, а консюмеры читают их асинхронно, сохраняя независимость и масштабируемость компонентов.

**Ключевые возможности:**
- Высокая пропускная способность (миллионы сообщений в секунду)
- Горизонтальная масштабируемость через партиционирование
- Долговременное хранение данных (дни/недели/бесконечно)
- Fault tolerance через репликацию
- Гарантии доставки (at-least-once, exactly-once)

**Пример создания простого продюсера:**
```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

KafkaProducer<String, String> producer = new KafkaProducer<>(props);
producer.send(new ProducerRecord<>("my-topic", "key", "value"));
```

***

## Вопрос 2

**Для чего используется Kafka?**

Kafka применяется для построения систем реального времени, где требуется надёжная передача, хранение и обработка потоков событий между сервисами. Основное назначение — разделение продюсеров и консюмеров данных через асинхронную очередь с долговременным хранением, что обеспечивает loose coupling, replay возможности и temporal decoupling компонентов.

**Типичные use cases:**
- **Event-Driven Architecture**: микросервисы обмениваются событиями, SAGA-паттерн для распределённых транзакций
- **Stream Processing**: агрегация метрик в реальном времени, fraud detection, real-time analytics
- **Log Aggregation**: централизованный сбор логов приложений, мониторинг и трейсинг
- **Data Integration**: ETL между системами, Change Data Capture (CDC), data lake ingestion
- **Messaging**: замена традиционных message brokers с преимуществом сохранения истории и replay

**Когда НЕ использовать:** request-response паттерн (лучше REST/gRPC), сложные routing rules (лучше RabbitMQ), маленький объём сообщений с простыми требованиями.

***

## Вопрос 3

**Как устроена архитектура Kafka?**

Архитектура Kafka — это распределённая система, состоящая из кластера брокеров, которые управляют топиками и партициями, координируемыми через ZooKeeper (или KRaft в новых версиях); продюсеры отправляют записи в топики, консюмеры читают через группы, а репликация обеспечивает отказоустойчивость через механизм лидер-реплик.

**Основные компоненты:**
- **Broker**: сервер Kafka, хранящий партиции, управляющий репликацией
- **Topic**: логическая категория сообщений, разделённая на партиции
- **Partition**: упорядоченная последовательность записей, unit of parallelism
- **ZooKeeper/KRaft**: координация кластера, метаданные, leader election
- **Producer**: клиент публикующий данные, выбирает партицию по ключу
- **Consumer Group**: распределённое чтение с назначением партиций участникам

**Архитектурная схема:**
```
Producer → Broker Cluster → Consumer
              ↓
         ZooKeeper/KRaft
         
Topic: orders
├─ Partition 0 (Leader: Broker1, Replicas: Broker2,3)
├─ Partition 1 (Leader: Broker2, Replicas: Broker1,3)
└─ Partition 2 (Leader: Broker3, Replicas: Broker1,2)
```

***

## Вопрос 4

**Что такое Topic в Kafka?**

Topic в Kafka — это именованная категория или канал, в который продюсеры публикуют записи, а из которого консюмеры их читают; topic логически представляет поток событий определённого типа и физически разделён на партиции для масштабируемости и параллелизма.

**Характеристики:** иммутабельность (append-only лог), retention policy (время/размер хранения или log compaction), партиционирование (N партиций, распределённых по брокерам), replication factor (количество копий для fault tolerance).

**Создание топика:**
```bash
kafka-topics.sh --create \
  --topic orders \
  --partitions 6 \
  --replication-factor 3 \
  --config retention.ms=604800000 \
  --bootstrap-server localhost:9092
```

**Naming conventions:** используйте domain.entity.action формат (`ecommerce.orders.created`), lowercase, избегайте точек в начале/конце.

***

## Вопрос 5

**Что такое Partition и зачем он нужен?**

Partition — это упорядоченная, неизменяемая последовательность записей внутри топика, которая является единицей параллелизма, масштабируемости и отказоустойчивости в Kafka; каждая партиция физически хранится на одном или нескольких брокерах (через репликацию) и гарантирует порядок записей внутри себя, но не между партициями.

**Зачем нужны:** горизонтальное масштабирование (throughput = sum партиций), упорядоченность по ключу (hash(key) % num_partitions), fault tolerance через репликацию.

**Партиционирование:**
```java
// С ключом — одинаковые ключи в одну партицию
producer.send(new ProducerRecord<>("orders", "user-123", orderJson));

// Без ключа — round-robin или sticky partitioner
producer.send(new ProducerRecord<>("orders", null, orderJson));
```

**Выбор количества:** `num_partitions = max(target_throughput / partition_throughput, max_concurrent_consumers)`. Начните с 6-12 партиций, увеличивайте по необходимости (нельзя уменьшить!).

***

## Вопрос 6

**Что такое Offset?**

Offset — это уникальный монотонно возрастающий идентификатор (последовательный номер) каждой записи внутри партиции, который позволяет консюмеру отслеживать свою позицию чтения и возвращаться к любому моменту в логе; offset специфичен для комбинации topic-partition-consumer-group и хранится либо в Kafka (__consumer_offsets topic), либо во внешнем хранилище.

**Типы offset:** current offset (текущая позиция), committed offset (сохранённая позиция для восстановления), log end offset/LEO (последняя запись), consumer lag (LEO - committed).

**Управление offset:**
```java
props.put("enable.auto.commit", "false");

while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord<String, String> record : records) {
        processRecord(record);
    }
    consumer.commitSync();  // Commit после обработки
}

// Сброс позиции
consumer.seekToBeginning(partitions);  // С начала
consumer.seek(partition, 12345L);      // Конкретный offset
```

**Auto offset reset:** `earliest` (с начала), `latest` (только новые, default), `none` (exception).

***

## Вопрос 7

**Что такое Producer в Kafka?**

Producer — это клиентское приложение, которое публикует (записывает) записи в топики Kafka, выбирая целевую партицию на основе ключа или кастомного partitioner'а, управляя батчированием, компрессией и гарантиями доставки через настройки acks, retries и idempotence.

**Жизненный цикл записи:** создание ProducerRecord → сериализация → выбор партиции → добавление в batch → отправка на брокер → ожидание acks → callback с результатом.

**Ключевые настройки:**
```java
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("acks", "all");                    // Гарантии доставки
props.put("enable.idempotence", "true");     // Exactly-once
props.put("batch.size", 16384);              // Батчирование
props.put("linger.ms", 10);                  // Задержка перед отправкой
props.put("compression.type", "lz4");        // Сжатие
```

**Отправка:**
```java
// Асинхронная с callback (рекомендуется)
producer.send(record, (metadata, exception) -> {
    if (exception != null) {
        log.error("Failed", exception);
    } else {
        log.info("Sent to partition {}, offset {}", 
            metadata.partition(), metadata.offset());
    }
});
```

***

## Вопрос 8

**Что такое Consumer в Kafka?**

Consumer — это клиентское приложение, которое подписывается на один или несколько топиков и читает записи из партиций, отслеживая свою позицию через offset'ы; консюмер обычно работает в составе consumer group для распределённой обработки, где каждая партиция назначается ровно одному консюмеру в группе.

**Polling loop:**
```java
Properties props = new Properties();
props.put("group.id", "my-consumer-group");
props.put("enable.auto.commit", "false");

KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(Arrays.asList("topic1", "topic2"));

while (true) {
    ConsumerRecords<String, String> records = 
        consumer.poll(Duration.ofMillis(100));
    
    for (ConsumerRecord<String, String> record : records) {
        processRecord(record);
    }
    consumer.commitSync();  // Commit после обработки
}
```

**Ключевые настройки:** `group.id`, `enable.auto.commit=false`, `auto.offset.reset=earliest`, `max.poll.records=500`, `max.poll.interval.ms=300000`, `session.timeout.ms=45000`.

**Pattern'ы:** at-least-once (commit после обработки), at-most-once (commit до обработки), exactly-once (transactional или idempotent processing).

***

## Вопрос 9

**Что такое Consumer Group?**

Consumer Group — это группа консюмеров с общим group.id, которые совместно читают топик, где Kafka автоматически распределяет партиции между членами группы так, что каждая партиция назначается ровно одному консюмеру в группе; это обеспечивает параллельную обработку, масштабируемость и fault tolerance через автоматический rebalance при изменении состава группы.

**Принципы работы:**
```
Топик с 6 партициями, группа из 3 консюмеров:
Consumer 1: [Partition 0, 1]
Consumer 2: [Partition 2, 3]
Consumer 3: [Partition 4, 5]

Если Consumer 2 падает (rebalance):
Consumer 1: [Partition 0, 1, 2]
Consumer 3: [Partition 3, 4, 5]

Правило: партиций >= консюмеров для эффективности
```

**Независимость групп:** каждая группа хранит свои offset'ы отдельно, разные группы могут читать одни данные независимо (один поток для analytics, другой для real-time processing).

**Мониторинг:**
```bash
kafka-consumer-groups.sh --bootstrap-server localhost:9092 \
  --group order-processing-group --describe
```

***

## Вопрос 10

**Что такое Broker в Kafka?**

Broker — это сервер Kafka, который хранит партиции топиков, обрабатывает запросы продюсеров и консюмеров, управляет репликацией и координирует с другими брокерами через контроллер для распределения нагрузки и обеспечения отказоустойчивости; кластер Kafka состоит из нескольких брокеров, каждый с уникальным broker.id.

**Обязанности:** хранение данных (партиции в log.dirs), обработка produce/fetch requests, репликация (leader/follower синхронизация, ISR управление), координация (один брокер — Controller для leader election).

**Конфигурация:**
```properties
broker.id=1
listeners=PLAINTEXT://broker1:9092
log.dirs=/var/kafka-logs
num.partitions=6
default.replication.factor=3
min.insync.replicas=2
log.retention.hours=168  # 7 дней
```

**Ключевые метрики:** UnderReplicatedPartitions, OfflinePartitionsCount, ActiveControllerCount (должен быть 1), BytesInPerSec/BytesOutPerSec, RequestHandlerAvgIdlePercent.

***

## Вопрос 11

**Что делает ZooKeeper в Kafka?**

ZooKeeper в Kafka — это сервис координации и хранения метаданных кластера, который управляет выбором контроллера, отслеживает состояние брокеров, хранит конфигурацию топиков и партиций, а также координирует выборы лидеров партиций; начиная с Kafka 2.8+ ZooKeeper постепенно заменяется встроенным протоколом KRaft (Kafka Raft) для упрощения архитектуры.

**Роли:** broker registry (ephemeral nodes для обнаружения сбоев), controller election (один брокер управляет partition assignment), topic configuration (метаданные, конфигурация), ACL и quotas, consumer group coordination (legacy, теперь offset'ы в __consumer_offsets).

**Переход на KRaft:**
```
KRaft преимущества:
✅ Проще развертывание (нет внешней зависимости)
✅ Быстрее metadata propagation
✅ Больше партиций поддерживается
✅ Упрощённый operational model

Kafka 4.0+: ZooKeeper deprecated
```

**Best practices:** 3 или 5 узлов (odd number), dedicated hardware, SSD для transaction logs, мониторинг latency (<10ms).

***

## Вопрос 12

**В чем разница между Key и Value в сообщениях?**

Key и Value в Kafka-записи — это два байтовых массива, где Value содержит полезную нагрузку сообщения (payload), а Key опционально определяет логический идентификатор или группировочный признак, который используется для выбора целевой партиции (через хеширование) и обеспечивает упорядоченность всех записей с одинаковым ключом внутри одной партиции.

**Назначение Key:** партиционирование (`partition = hash(key) % num_partitions`, одинаковые ключи → одна партиция → порядок сохранён), упорядоченность (события user-123 обрабатываются по порядку), log compaction (хранится только последнее значение для каждого key, tombstone при value=null).

**Примеры:**
```java
// Без ключа — round-robin
new ProducerRecord<>("topic", null, "message");

// С ключом — hash партиционирование
new ProducerRecord<>("topic", "user-123", orderJson);

// Tombstone — удаление в compacted topic
new ProducerRecord<>("topic", "user-123", null);
```

**Trade-offs:** с Key (упорядоченность, compaction, но риск hot partitions), без Key (равномерное распределение, выше throughput, но нет порядка).

***

## Вопрос 13

**Что такое Replication в Kafka?**

Replication в Kafka — это механизм дублирования каждой партиции на несколько брокеров (определяется replication.factor), где одна реплика является лидером (обрабатывает все read/write), а остальные — фолловерами (синхронизируются с лидером), что обеспечивает отказоустойчивость и доступность данных при падении брокеров без потери сообщений.

**Архитектура:**
```
Topic: orders, Partitions: 3, RF: 3

Partition 0: Leader=Broker1, Followers=Broker2,3
Partition 1: Leader=Broker2, Followers=Broker1,3
Partition 2: Leader=Broker3, Followers=Broker1,2

Если Broker1 падает:
- Partition 0: новый leader из ISR (Broker2 или 3)
- Partition 1,2: followers пропадают, но leader работает
```

**Роли:** Leader (принимает produce/fetch, координирует ISR, записывает в local log), Follower (fetch от leader, копирует в local log, не обслуживает клиентов, кандидат на leader).

**Конфигурация:**
```properties
replication.factor=3
min.insync.replicas=2
replica.lag.time.max.ms=30000
```

**Failover:** ZooKeeper обнаруживает потерю → Controller инициирует election → новый leader из ISR → metadata обновляется → клиенты переключаются.

***

## Вопрос 14

**Что такое ISR (In-Sync Replica)?**

ISR (In-Sync Replicas) — это список реплик партиции, которые находятся в синхронизированном состоянии с лидером (не отстают больше чем на replica.lag.time.max.ms) и могут быть выбраны новым лидером при failover; продюсер с acks=all ждёт подтверждения от всех реплик в ISR перед подтверждением записи, что обеспечивает durability без потери данных при падении лидера.

**Критерии включения:** регулярные fetch requests к leader, отставание < replica.lag.time.max.ms (default 30 сек), успешный heartbeat с ZooKeeper/Controller.

**Исключение:** не fetch'ит вовремя, отстаёт больше порога, недоступна, disk I/O проблемы.

**Связь с acks:**
```java
props.put("acks", "all");  // Producer ждёт подтверждения от всех ISR

RF=3, ISR=[Broker1,2,3]: ждём 3 acks
RF=3, ISR=[Broker1,2]:   ждём 2 acks (быстрее, меньше durability)
```

**min.insync.replicas:**
```properties
min.insync.replicas=2

RF=3, min.insync.replicas=2:
✅ ISR=3 → produce OK
✅ ISR=2 → produce OK
❌ ISR=1 → produce FAIL (NotEnoughReplicas)
```

**Best practice:** `min.insync.replicas = replication.factor / 2 + 1`.

***

## Вопрос 15

**Что такое Leader и Follower партиций?**

Leader и Follower — это роли реплик партиции в Kafka, где Leader обрабатывает все запросы на чтение и запись от продюсеров и консюмеров, а Follower'ы пассивно реплицируют данные с лидера для обеспечения отказоустойчивости; при падении лидера один из follower'ов из ISR автоматически избирается новым лидером контроллером кластера.

**Обязанности Leader:** обработка produce requests (принимает записи, координирует репликацию), обработка fetch requests (отдаёт данные консюмерам и follower'ам), управление ISR (отслеживает отставание, добавляет/удаляет из ISR), high watermark management (определяет committed записи).

**Обязанности Follower:** репликация (постоянный fetch от leader, копирование в local log), кандидат на leader (должен быть в ISR), passive role (НЕ обслуживает продюсеров/консюмеров по умолчанию).

**Leader Election:**
```
Триггеры: падение broker с leader, graceful shutdown, preferred election

Процесс:
1. Controller обнаруживает проблему
2. Выбирает нового leader из ISR
3. Обновляет метаданные в ZooKeeper
4. Уведомляет брокеры
5. Клиенты получают обновлённые metadata
```

**Конфигурация:**
```properties
auto.leader.rebalance.enable=true
unclean.leader.election.enable=false  # Не выбирать вне ISR
```

***

## Вопрос 16

**Что такое Producer Acknowledgement (acks)?**

Producer Acknowledgement (acks) — это настройка продюсера, определяющая сколько подтверждений от брокеров должен получить продюсер перед тем, как считать запись успешно отправленной; управляет балансом между durability (надёжностью доставки) и latency (задержкой), влияя на риск потери данных при сбоях брокеров.

**Значения acks:**
- **acks=0**: продюсер не ждёт ответа (fire-and-forget), максимальная скорость, высокий риск потерь
- **acks=1**: ждёт подтверждения только от лидера, баланс между скоростью и надёжностью, риск потери при падении лидера до репликации
- **acks=all (или -1)**: ждёт подтверждения от всех реплик в ISR, максимальная надёжность, большая latency

**Конфигурация:**
```java
props.put("acks", "all");  // Рекомендуется для критичных данных
props.put("min.insync.replicas", "2");  // На broker/topic level
```

**Trade-off:** acks=0 (высокий throughput, потери возможны) → acks=1 (баланс) → acks=all (durability, latency выше).

***

## Вопрос 17

**В чем разница между acks=0, acks=1 и acks=all?**

Параметры acks определяют уровень гарантий доставки записи: acks=0 означает отсутствие ожидания подтверждений (максимальная скорость, возможны потери при любых сбоях), acks=1 требует подтверждения только от лидера партиции (средняя надёжность, риск потери при падении лидера до репликации на follower'ы), acks=all гарантирует запись на всех репликах из ISR перед подтверждением (максимальная durability, используется с min.insync.replicas для защиты от потерь).

**Сравнение:**

| acks | Durability | Latency | Throughput | Риск потери |
|------|-----------|---------|------------|-------------|
| 0 | Нет гарантий | Минимальная | Максимальный | Высокий |
| 1 | Средняя | Средняя | Высокий | Средний (до репликации) |
| all | Максимальная | Высокая | Ниже | Минимальный (с ISR) |

**Сценарии:**
```
acks=0: метрики, логи (некритичные данные)
acks=1: большинство use cases (баланс)
acks=all: финансы, заказы, критичные транзакции
```

**Защита с acks=all:**
```properties
acks=all
min.insync.replicas=2
replication.factor=3

Гарантия: минимум 2 реплики подтвердили → данные не потеряются
```

***

## Вопрос 18

**Что такое Idempotent Producer?**

Idempotent Producer — это продюсер с включённой идемпотентностью (enable.idempotence=true), который гарантирует exactly-once семантику записи в рамках одной сессии продюсера, предотвращая дублирование сообщений при сетевых ошибках и retry; Kafka присваивает каждому продюсеру уникальный Producer ID (PID) и sequence number для каждой записи, позволяя брокеру отфильтровывать дубли.

**Как работает:**
```
1. Producer получает PID от брокера
2. Каждое сообщение получает sequence number (per partition)
3. Broker отслеживает последний sequence number
4. Duplicate requests (same PID + seq) отклоняются
5. Out-of-order sequence → OutOfOrderSequenceException
```

**Конфигурация:**
```java
props.put("enable.idempotence", "true");  // Автоматически устанавливает:
// acks=all
// max.in.flight.requests.per.connection=5
// retries=Integer.MAX_VALUE
```

**Limitations:** идемпотентность только в рамках одной producer session (после перезапуска — новый PID), работает per-partition (не across partitions), не защищает от application-level дублей (нужен business key).

**Use case:** предотвращение дублей при network failures, retry без риска duplicate writes, exactly-once внутри Kafka.

***

## Вопрос 19

**Что такое Transactional Producer?**

Transactional Producer — это продюсер с включённым transactional.id, который поддерживает атомарные мульти-партиционные записи и координацию с консюмерами для обеспечения exactly-once семантики end-to-end (чтение → обработка → запись); транзакции позволяют группировать несколько записей в разные топики/партиции как единую атомарную операцию с commit/abort семантикой.

**Возможности:** атомарная запись в несколько партиций/топиков, координация с consumer offset'ами (commit offset как часть транзакции), exactly-once processing (read-process-write), автоматический abort при сбоях.

**Конфигурация и использование:**
```java
// Producer
props.put("transactional.id", "my-transactional-id");
props.put("enable.idempotence", "true");

producer.initTransactions();

try {
    producer.beginTransaction();
    
    producer.send(new ProducerRecord<>("topic1", "key", "value1"));
    producer.send(new ProducerRecord<>("topic2", "key", "value2"));
    
    // Commit consumer offset как часть транзакции
    producer.sendOffsetsToTransaction(offsets, groupMetadata);
    
    producer.commitTransaction();
} catch (Exception e) {
    producer.abortTransaction();
}

// Consumer (read committed)
props.put("isolation.level", "read_committed");
```

**Use case:** Kafka Streams, exactly-once ETL pipelines, координация между топиками, atomic database + Kafka writes.

***

## Вопрос 20

**В чем разница между at-least-once, at-most-once и exactly-once?**

Это три семантики доставки сообщений, определяющие гарантии обработки: at-least-once гарантирует доставку минимум один раз (возможны дубли при retry), at-most-once гарантирует максимум одну доставку (возможны потери), exactly-once гарантирует ровно одну обработку без дублей и потерь (требует idempotent/transactional producers и координации с consumer offset'ами).

**At-Least-Once (default):**
```java
// Commit offset ПОСЛЕ обработки
while (true) {
    records = consumer.poll(Duration.ofMillis(100));
    for (record : records) {
        processRecord(record);  // Может упасть ДО commit
    }
    consumer.commitSync();  // Commit после успеха
}
// Если crash до commit → reprocess → дубли возможны
```

**At-Most-Once:**
```java
// Commit offset ДО обработки
while (true) {
    records = consumer.poll(Duration.ofMillis(100));
    consumer.commitSync();  // Commit сразу
    for (record : records) {
        processRecord(record);  // Может упасть
    }
}
// Если crash после commit → потеря сообщений
```

**Exactly-Once:**
```java
// Transactional approach
props.put("isolation.level", "read_committed");
producer.initTransactions();

while (true) {
    records = consumer.poll(Duration.ofMillis(100));
    producer.beginTransaction();
    
    for (record : records) {
        process(record);
        producer.send(outputRecord);
    }
    
    producer.sendOffsetsToTransaction(offsets, groupMetadata);
    producer.commitTransaction();
}
// Атомарность: обработка + offset commit
```

**Выбор:** at-least-once (большинство случаев, требует idempotent processing), at-most-once (редко, только для некритичных данных), exactly-once (финансы, критичные транзакции, требует transactional API).

***

## Вопрос 21

**Что такое Consumer Offset?**

Consumer Offset — это позиция (номер записи) в партиции топика, до которой консюмер успешно обработал данные; offset сохраняется в специальном внутреннем топике __consumer_offsets для каждой комбинации consumer-group + topic + partition, позволяя консюмеру продолжить чтение с последней обработанной позиции после перезапуска или rebalance.

**Типы:** current offset (текущая позиция в poll), committed offset (сохранённый в __consumer_offsets для recovery), log end offset/LEO (последняя запись в партиции).

**Commit стратегии:**
```java
// Автоматический commit (не рекомендуется для критичных данных)
props.put("enable.auto.commit", "true");
props.put("auto.commit.interval.ms", "5000");

// Ручной синхронный (блокирующий)
consumer.commitSync();

// Ручной асинхронный (неблокирующий)
consumer.commitAsync((offsets, exception) -> {
    if (exception != null) log.error("Commit failed", exception);
});

// Commit конкретных offset'ов
Map<TopicPartition, OffsetAndMetadata> offsets = new HashMap<>();
offsets.put(partition, new OffsetAndMetadata(record.offset() + 1));
consumer.commitSync(offsets);
```

**Best practice:** enable.auto.commit=false, commit после успешной обработки (at-least-once), хранить offset в той же транзакции что и результат (exactly-once).

***

## Вопрос 22

**Как работает Consumer Poll?**

Consumer Poll — это основной метод получения записей от Kafka, который выполняет blocking fetch с брокеров в течение указанного timeout, возвращает batch записей (до max.poll.records), автоматически отправляет heartbeat'ы для поддержания членства в группе и триггерит rebalance при необходимости; poll должен вызываться регулярно (в пределах max.poll.interval.ms), иначе консюмер считается мёртвым.

**Механизм работы:**
```
1. consumer.poll(Duration.ofMillis(100)) вызван
2. Send heartbeat (если пора по heartbeat.interval.ms)
3. Check rebalance status
4. Fetch records от assigned партиций
5. Deserialize key/value
6. Return ConsumerRecords (до max.poll.records)
```

**Параметры:**
```java
props.put("max.poll.records", 500);        // Сколько записей за poll
props.put("max.poll.interval.ms", 300000); // 5 минут на обработку batch
props.put("fetch.min.bytes", 1);           // Минимум данных для возврата
props.put("fetch.max.wait.ms", 500);       // Максимальное ожидание данных
```

**Типичный loop:**
```java
while (true) {
    ConsumerRecords<String, String> records = 
        consumer.poll(Duration.ofMillis(100));
    
    for (ConsumerRecord<String, String> record : records) {
        // Обработка должна завершиться в пределах max.poll.interval.ms
        processRecord(record);
    }
    
    consumer.commitSync();
}
```

**Important:** долгая обработка в poll loop → риск rebalance, используйте отдельные потоки для heavy processing или уменьшите max.poll.records.

***

## Вопрос 23

**Что такое Offset Commit?**

Offset Commit — это операция сохранения текущей позиции консюмера в партиции во внутренний топик __consumer_offsets, что позволяет возобновить чтение с этой позиции после перезапуска консюмера или rebalance; commit может быть автоматическим (по интервалу) или ручным (после успешной обработки), и определяет семантику доставки (at-least-once vs at-most-once).

**Стратегии commit:**
```java
// 1. Auto commit (простой, but risks)
props.put("enable.auto.commit", "true");
props.put("auto.commit.interval.ms", "5000");
// Риск: commit может произойти до завершения обработки

// 2. Manual sync commit (blocking, надёжный)
props.put("enable.auto.commit", "false");
for (record : records) {
    processRecord(record);
}
consumer.commitSync();  // Блокируется до подтверждения

// 3. Manual async commit (неблокирующий)
consumer.commitAsync((offsets, exception) -> {
    if (exception != null) {
        log.error("Commit failed", exception);
        // Retry logic
    }
});

// 4. Commit specific offsets
Map<TopicPartition, OffsetAndMetadata> offsets = Map.of(
    partition, new OffsetAndMetadata(record.offset() + 1, "metadata")
);
consumer.commitSync(offsets);
```

**Timing влияет на семантику:**
```
Commit ДО обработки:
✅ At-most-once (без дублей)
❌ Риск потери при crash

Commit ПОСЛЕ обработки:
✅ At-least-once (без потерь)
❌ Риск дублей при crash до commit
```

**Best practice:** ручной commit после успешной обработки, sync commit для критичных данных, async для throughput, обрабатывать CommitFailedException.

***

## Вопрос 24

**Что такое Auto Offset Reset?**

Auto Offset Reset — это настройка консюмера (auto.offset.reset), определяющая поведение при отсутствии сохранённого committed offset или когда committed offset больше не существует (например, данные удалены по retention policy); возможные значения: earliest (читать с начала партиции), latest (читать только новые сообщения), none (выбросить exception).

**Значения:**
```properties
# Читать с самого начала партиции
auto.offset.reset=earliest

# Читать только новые сообщения (default)
auto.offset.reset=latest

# Выбросить exception если offset не найден
auto.offset.reset=none
```

**Когда срабатывает:**
```
1. Новая consumer group (нет committed offset)
2. Committed offset expired (удалён по offsets.retention.minutes)
3. Committed offset за пределами available range
```

**Use cases:**
```
earliest:
- Reprocessing всех данных
- Data recovery
- Новые consumer'ы должны видеть всю историю

latest:
- Real-time processing (только новые события)
- Не интересует история
- Default для большинства случаев

none:
- Strict control (fail fast на проблемы)
- Production с мониторингом
```

**Programmatic reset:**
```java
// Явный сброс на начало
consumer.seekToBeginning(partitions);

// Сброс на конец
consumer.seekToEnd(partitions);

// Сброс на timestamp
long timestamp = Instant.now().minus(7, ChronoUnit.DAYS).toEpochMilli();
consumer.offsetsForTimes(timestampsToSearch);
```

***

## Вопрос 25

**Какие стратегии балансировки Consumer Group существуют?**

Стратегии балансировки (Partition Assignment Strategies) определяют алгоритм распределения партиций между консюмерами группы при subscribe: RangeAssignor назначает непрерывные диапазоны партиций каждого топика (default, может быть неравномерным), RoundRobinAssignor равномерно чередует партиции между консюмерами, StickyAssignor минимизирует перераспределение при rebalance, CooperativeStickyAssignor избегает полной остановки обработки во время rebalance через инкрементальный подход.

**RangeAssignor (default):**
```
Topic с 7 партициями, 3 консюмера:
Consumer1: [0,1,2]  (ceil(7/3))
Consumer2: [3,4]
Consumer3: [5,6]

Для 2 топиков (T1, T2 по 7 партиций):
Consumer1: [T1:0-2, T2:0-2]  (более загружен!)
Consumer2: [T1:3-4, T2:3-4]
Consumer3: [T1:5-6, T2:5-6]
```

**RoundRobinAssignor:**
```
Равномерное распределение по всем топикам:
Consumer1: [T1:0, T1:3, T1:6, T2:2, T2:5]
Consumer2: [T1:1, T1:4, T2:0, T2:3, T2:6]
Consumer3: [T1:2, T1:5, T2:1, T2:4]
```

**StickyAssignor:**
```
Минимизирует изменения при rebalance:
Initial: C1:[0,1], C2:[2,3], C3:[4,5]
C2 падает: C1:[0,1,2], C3:[3,4,5]  (минимум перемещений)
```

**CooperativeStickyAssignor (рекомендуется):**
```
Incremental rebalance без stop-the-world:
- Revoke только перемещаемые партиции
- Остальные продолжают обработку
- Меньше downtime
```

**Конфигурация:**
```java
props.put("partition.assignment.strategy", 
    "org.apache.kafka.clients.consumer.CooperativeStickyAssignor");
```

***

## Вопрос 26

**Что такое Rebalance и когда он происходит?**

Rebalance — это процесс перераспределения партиций между консюмерами группы, который происходит при изменении состава группы (добавление/удаление консюмеров) или изменении подписок на топики; во время eager rebalance все консюмеры останавливают обработку (stop-the-world), возвращают свои партиции и получают новое назначение, что временно блокирует чтение данных из топика.

**Триггеры rebalance:**
```
1. Новый консюмер join в группу
2. Консюмер покидает группу (graceful shutdown)
3. Консюмер считается мёртвым:
   - Не вызывает poll() в течение max.poll.interval.ms
   - Не отправляет heartbeat в течение session.timeout.ms
4. Изменение подписки (subscribe на другие топики)
5. Изменение количества партиций в топике
```

**Типы rebalance:**
```
Eager (старый):
1. Все консюмеры stop processing
2. Все revoke свои партиции
3. Новое назначение
4. Все resume processing
→ Stop-the-world, высокий downtime

Cooperative/Incremental (новый):
1. Идентифицируем партиции для перемещения
2. Revoke только их
3. Остальные продолжают работу
4. Assign перемещённые партиции
→ Минимальный downtime
```

**Настройки:**
```java
props.put("session.timeout.ms", "45000");        // Heartbeat timeout
props.put("heartbeat.interval.ms", "3000");      // Heartbeat frequency
props.put("max.poll.interval.ms", "300000");     // 5 мин на обработку
props.put("partition.assignment.strategy", 
    "CooperativeStickyAssignor");  // Incremental rebalance
```

**Минимизация rebalance:**
```
✅ Используйте CooperativeStickyAssignor
✅ Static membership (group.instance.id для rolling restarts)
✅ Быстрая обработка (в пределах max.poll.interval.ms)
✅ Stable infrastructure
❌ Избегайте частых deploy'ов
❌ Не увеличивайте консюмеров больше чем партиций
```

***

## Вопрос 27

**Что такое delivery.timeout.ms и linger.ms?**

delivery.timeout.ms — это максимальное время, которое продюсер ждёт доставки записи (включая retry и подтверждение), после чего запись считается failed; linger.ms — это время задержки перед отправкой batch на брокер для накопления большего количества записей в одном запросе, что увеличивает throughput за счёт небольшой латентности.

**delivery.timeout.ms:**
```java
props.put("delivery.timeout.ms", "120000");  // 2 минуты (default)

// Должно быть >= linger.ms + request.timeout.ms
// Включает время на:
// - Батчирование (linger.ms)
// - Retry (retries * retry.backoff.ms)
// - Ожидание acks (request.timeout.ms)

Если превышено → TimeoutException
```

**linger.ms:**
```java
props.put("linger.ms", "10");  // Ждём 10ms перед отправкой

Работа:
1. Запись добавлена в batch
2. Ждём linger.ms или пока batch.size заполнится
3. Отправляем batch

linger.ms=0:  немедленная отправка, низкая latency, меньше throughput
linger.ms=10: батчирование, выше latency, больше throughput
```

**Trade-off:**
```
Low latency (real-time):
linger.ms=0
delivery.timeout.ms=30000

High throughput (batch):
linger.ms=20
batch.size=32768
compression.type=lz4
```

**Связь с другими параметрами:**
```
delivery.timeout.ms >= linger.ms + request.timeout.ms

retries учитываются в delivery.timeout.ms:
Если retry занимает много времени → может превысить delivery.timeout
```

***

## Вопрос 28

**Что такое batch.size и buffer.memory?**

batch.size — это максимальный размер batch (в байтах) записей для одной партиции, который продюсер накапливает перед отправкой на брокер для повышения throughput через амортизацию сетевых запросов; buffer.memory — это общий объём памяти (в байтах), доступный продюсеру для буферизации записей перед отправкой, исчерпание которого блокирует send() до max.block.ms или выбрасывает exception.

**batch.size:**
```java
props.put("batch.size", 16384);  // 16KB default

Поведение:
- Batch отправляется когда:
  1. Достигнут batch.size
  2. Истёк linger.ms
  3. Flush() вызван

Влияние:
Больше batch.size:
✅ Выше throughput (меньше requests)
✅ Лучше compression
❌ Больше memory usage
❌ Выше latency

Меньше batch.size:
✅ Ниже latency
❌ Ниже throughput
```

**buffer.memory:**
```java
props.put("buffer.memory", 33554432);  // 32MB default

Работа:
1. send() добавляет запись в buffer
2. Background thread отправляет batch'и
3. Если buffer полон:
   - Блокируется на max.block.ms
   - Затем TimeoutException

Причины заполнения:
- Продюсер быстрее отправляет чем брокер принимает
- Network проблемы (медленная отправка)
- Broker перегружен
```

**Оптимизация:**
```java
// High throughput setup
props.put("batch.size", 32768);       // 32KB
props.put("linger.ms", 20);           // Больше батчирования
props.put("buffer.memory", 67108864); // 64MB
props.put("compression.type", "lz4"); // Compression

// Low latency setup
props.put("batch.size", 1024);        // 1KB
props.put("linger.ms", 0);            // Немедленно
props.put("buffer.memory", 16777216); // 16MB
```

**Мониторинг:**
```
Метрики:
- buffer-available-bytes: свободная память
- bufferpool-wait-time: время ожидания памяти
- batch-size-avg: средний размер batch
```

***

## Вопрос 29

**Что такое max.poll.records и max.poll.interval.ms?**

max.poll.records — это максимальное количество записей, которое consumer.poll() возвращает за один вызов для контроля размера batch и времени обработки; max.poll.interval.ms — это максимальное время между вызовами poll(), после которого консюмер считается мёртвым и исключается из группы с триггером rebalance, что требует завершения обработки batch в указанный срок.

**max.poll.records:**
```java
props.put("max.poll.records", 500);  // Default

Влияние:
Больше max.poll.records:
✅ Выше throughput (меньше poll() calls)
✅ Меньше overhead
❌ Дольше обработка batch
❌ Риск превысить max.poll.interval.ms

Меньше max.poll.records:
✅ Быстрее обработка
✅ Меньше риск timeout
❌ Больше poll() calls
❌ Ниже throughput
```

**max.poll.interval.ms:**
```java
props.put("max.poll.interval.ms", 300000);  // 5 минут default

Работа:
while (true) {
    records = consumer.poll(Duration.ofMillis(100));
    // Обработка должна завершиться за max.poll.interval.ms
    processRecords(records);  // Если > 5 мин → rebalance!
}

Если превышено:
1. Консюмер считается мёртвым
2. Триггерится rebalance
3. Партиции переназначаются другим консюмерам
```

**Выбор значений:**
```
Формула:
max.poll.interval.ms >= max.poll.records * avg_processing_time_per_record

Пример:
avg_processing_time = 100ms
max.poll.records = 500
min max.poll.interval.ms = 500 * 100ms = 50 секунд
→ Установите 120000ms (2 минуты) с запасом

Долгая обработка:
max.poll.records=100
max.poll.interval.ms=600000  (10 минут)
```

**Best practice:**
```
✅ Обрабатывайте быстро или используйте отдельные потоки
✅ Уменьшите max.poll.records для долгой обработки
✅ Мониторьте processing time
❌ Не делайте blocking I/O в poll loop
❌ Не делайте heavy computation синхронно
```

***

## Вопрос 30

**Что такое Consumer Lag?**

Consumer Lag — это разница между последним доступным offset в партиции (log end offset/LEO) и текущим committed offset консюмера, показывающая насколько консюмер отстаёт от реального потока данных; высокий lag указывает на проблемы производительности консюмера (медленная обработка, недостаточно ресурсов, rebalance) или аномально высокую нагрузку продюсеров.

**Расчёт:**
```
Consumer Lag = Log End Offset - Current Committed Offset

Пример:
Partition 0:
- LEO (последняя запись): 10,000
- Committed offset consumer group A: 9,500
- Lag: 500 сообщений

Нулевой lag: консюмер real-time
Растущий lag: консюмер не успевает
```

**Мониторинг lag:**
```bash
# CLI
kafka-consumer-groups.sh --bootstrap-server localhost:9092 \
  --group my-group --describe

# Output:
TOPIC     PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG
orders    0          9500            10000           500
orders    1          8700            8700            0
```

**Причины высокого lag:**
```
1. Медленная обработка:
   - Heavy computation
   - Blocking I/O (database, external API)
   - Недостаточно ресурсов (CPU, memory)

2. Недостаточно консюмеров:
   - Партиций больше чем консюмеров
   - Нужно horizontal scaling

3. Частые rebalance:
   - Нестабильность консюмеров
   - Превышение max.poll.interval.ms

4. Высокая нагрузка продюсеров:
   - Spike в трафике
   - Нужно больше partition'ов
```

**Решения:**
```
✅ Добавить консюмеров (до количества партиций)
✅ Оптимизировать обработку (async, batch DB operations)
✅ Увеличить max.poll.records для throughput
✅ Добавить партиций (для будущего, текущие не помогут)
✅ Scale вертикально (больше CPU/RAM)
❌ Не игнорируйте растущий lag
```

**Алерты:**
```
Warning: lag > 1000 сообщений
Critical: lag > 10000 или растёт >5 минут
```

***

## Вопрос 31

**Что такое Retention Policy?**

Retention Policy — это настройка определяющая как долго Kafka хранит данные в топике перед удалением, основанная на времени (retention.ms) или размере (retention.bytes); после превышения лимита старые сегменты лога удаляются, освобождая дисковое пространство, что позволяет балансировать между доступностью исторических данных и стоимостью хранения.

**Типы retention:**
```properties
# Time-based (default)
retention.ms=604800000  # 7 дней (default)
# или
retention.hours=168
retention.minutes=10080

# Size-based (per partition)
retention.bytes=1073741824  # 1GB

# Оба одновременно: первый достигнутый лимит
retention.ms=604800000
retention.bytes=10737418240  # 10GB
```

**Как работает:**
```
1. Kafka делит партицию на segments (segment.bytes)
2. Активный segment: текущие записи
3. Closed segments: старые, могут быть удалены
4. Background thread проверяет retention (log.retention.check.interval.ms)
5. Удаляет segments старше retention.ms или превышающие retention.bytes
```

**Конфигурация:**
```bash
# Topic-level (приоритет над broker default)
kafka-configs.sh --alter \
  --entity-type topics --entity-name orders \
  --add-config retention.ms=2592000000 \  # 30 дней
  --bootstrap-server localhost:9092

# Broker-level (default для новых топиков)
# server.properties
log.retention.hours=168
log.retention.bytes=-1  # Unlimited
```

**Use cases:**
```
Короткий retention (часы/дни):
- Temporary queues
- Real-time processing
- High-volume logs

Средний retention (недели):
- Standard event logs
- Audit trails
- Analytics pipelines

Долгий retention (месяцы/forever):
- Compliance data
- Event sourcing
- Data lake источник

Infinite retention:
retention.ms=-1  # + cleanup.policy=compact
```

**Trade-offs:**
```
Долгий retention:
✅ Replay historical data
✅ Disaster recovery
✅ Late consumers
❌ Больше disk usage
❌ Выше стоимость storage

Короткий retention:
✅ Меньше disk
✅ Дешевле
❌ Нет истории
❌ Нельзя replay старые данные
```

***

## Вопрос 32

**Что такое Log Compaction?**

Log Compaction — это retention policy (cleanup.policy=compact), при которой Kafka сохраняет только последнюю запись для каждого уникального ключа, удаляя устаревшие версии, но не удаляя ключи полностью; это позволяет использовать топик как changelog или материализованную таблицу состояний с возможностью восстановления полного snapshot через чтение всего compacted лога.

**Как работает:**
```
Исходные записи:
key=user-1, value={name:"Alice", age:25}
key=user-2, value={name:"Bob", age:30}
key=user-1, value={name:"Alice", age:26}  ← обновление
key=user-3, value={name:"Charlie", age:35}
key=user-2, value=null  ← tombstone (удаление)

После compaction:
key=user-1, value={name:"Alice", age:26}  ← последняя версия
key=user-3, value={name:"Charlie", age:35}
(user-2 удалён tombstone)

Гарантия: последнее состояние для каждого ключа сохранено
```

**Конфигурация:**
```properties
# Включить compaction
cleanup.policy=compact
# Или оба (time + compaction)
cleanup.policy=compact,delete

# Compaction параметры
min.cleanable.dirty.ratio=0.5  # Когда запускать compaction
segment.ms=604800000           # Закрывать segment каждые 7 дней
min.compaction.lag.ms=0        # Минимальное время до compaction
delete.retention.ms=86400000   # Хранить tombstone 1 день
```

**Use cases:**
```
✅ Event sourcing (последнее состояние entity)
✅ CDC (Change Data Capture) из БД
✅ Cache materialization (Redis-like в Kafka)
✅ Configuration management
✅ Kafka Streams state stores

❌ НЕ подходит для:
- Audit logs (нужна полная история)
- Time-series data
- События без ключа
```

**Tombstone (удаление):**
```java
// Отправка tombstone для удаления ключа
producer.send(new ProducerRecord<>("users", "user-123", null));

// После delete.retention.ms key полностью удалится
```

**Восстановление состояния:**
```java
// Новый консюмер читает compacted topic с начала
consumer.seekToBeginning(partitions);

// Получает snapshot всех текущих состояний
while (true) {
    records = consumer.poll(Duration.ofMillis(100));
    for (record : records) {
        cache.put(record.key(), record.value());  // Rebuild state
    }
}
```

**Важные нюансы:**
```
- Compaction асинхронный (не instant)
- Активный segment не compacted
- Порядок в партиции сохраняется
- Требует key (без key не работает)
- Tombstone должен быть явным (value=null)
```

***

## Вопрос 33

**Какие форматы сериализации поддерживает Kafka?**

Kafka работает с байтовыми массивами и не налагает ограничений на формат данных, но требует сериализаторов (для продюсера) и десериализаторов (для консюмера) для преобразования объектов в байты и обратно; встроенные сериализаторы включают String, Integer, Long, ByteArray, а популярные внешние форматы — JSON, Avro, Protobuf, Thrift — требуют соответствующих библиотек и часто используются со Schema Registry для версионирования и валидации.

**Встроенные serializers:**
```java
// String (UTF-8)
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

// Integer, Long, Double
props.put("key.serializer", "org.apache.kafka.common.serialization.IntegerSerializer");

// ByteArray (raw bytes)
props.put("value.serializer", "org.apache.kafka.common.serialization.ByteArraySerializer");
```

**JSON (с Jackson):**
```java
public class JsonSerializer<T> implements Serializer<T> {
    private ObjectMapper objectMapper = new ObjectMapper();
    
    @Override
    public byte[] serialize(String topic, T data) {
        try {
            return objectMapper.writeValueAsBytes(data);
        } catch (Exception e) {
            throw new SerializationException("Error serializing", e);
        }
    }
}

// Использование
props.put("value.serializer", "com.example.JsonSerializer");
```

**Avro (с Schema Registry):**
```java
// Maven dependency: io.confluent:kafka-avro-serializer

props.put("value.serializer", 
    "io.confluent.kafka.serializers.KafkaAvroSerializer");
props.put("schema.registry.url", "http://localhost:8081");

// Avro schema (User.avsc)
{
  "type": "record",
  "name": "User",
  "fields": [
    {"name": "id", "type": "long"},
    {"name": "name", "type": "string"},
    {"name": "email", "type": "string"}
  ]
}
```

**Protobuf:**
```java
// Maven: io.confluent:kafka-protobuf-serializer

props.put("value.serializer", 
    "io.confluent.kafka.serializers.protobuf.KafkaProtobufSerializer");
```

**Сравнение форматов:**

| Формат | Размер | Скорость | Schema | Читаемость | Эволюция |
|--------|--------|----------|--------|------------|----------|
| JSON | Большой | Медленно | Нет | ✅ Высокая | Слабая |
| Avro | Маленький | Быстро | ✅ Да | ❌ Binary | ✅ Сильная |
| Protobuf | Маленький | Быстро | ✅ Да | ❌ Binary | ✅ Сильная |
| String | Средний | Быстро | Нет | ✅ Высокая | Нет |

**Best practices:**
```
✅ Используйте Schema Registry для Avro/Protobuf (версионирование)
✅ JSON для простых случаев и debugging
✅ Avro/Protobuf для production (компактность, schema evolution)
❌ Избегайте Java Serialization (медленно, unsafe)
```

***

## Вопрос 34

**Что такое Schema Registry?**

Schema Registry — это сервис (обычно Confluent Schema Registry), который централизованно хранит, версионирует и валидирует схемы данных (Avro, Protobuf, JSON Schema) для топиков Kafka, позволяя продюсерам и консюмерам эволюционировать форматы данных с гарантиями совместимости (backward, forward, full compatibility) без breaking changes и явной координации между сервисами.

**Как работает:**
```
Producer:
1. Сериализует данные согласно схеме
2. Отправляет схему в Registry (если новая)
3. Получает schema ID
4. Записывает в Kafka: [magic byte][schema ID][data]

Consumer:
1. Читает запись из Kafka
2. Извлекает schema ID
3. Запрашивает схему из Registry (кэшируется)
4. Десериализует данные
```

**Режимы совместимости:**
```
BACKWARD (default):
- Новые consumers читают старые данные
- Можно удалять поля, добавлять с defaults
- Use case: добавление новых полей

FORWARD:
- Старые consumers читают новые данные
- Можно удалять поля с defaults, добавлять любые
- Use case: удаление deprecated полей

FULL (BACKWARD + FORWARD):
- Оба направления совместимы
- Только добавление/удаление с defaults
- Use case: strict compatibility

NONE:
- Без проверки
- Use case: полный контроль версий
```

**Конфигурация:**
```java
// Producer
props.put("value.serializer", 
    "io.confluent.kafka.serializers.KafkaAvroSerializer");
props.put("schema.registry.url", "http://localhost:8081");

// Consumer
props.put("value.deserializer", 
    "io.confluent.kafka.serializers.KafkaAvroDeserializer");
props.put("schema.registry.url", "http://localhost:8081");
props.put("specific.avro.reader", "true");
```

**Schema evolution пример:**
```
Version 1:
{
  "name": "User",
  "fields": [
    {"name": "id", "type": "long"},
    {"name": "name", "type": "string"}
  ]
}

Version 2 (BACKWARD compatible):
{
  "name": "User",
  "fields": [
    {"name": "id", "type": "long"},
    {"name": "name", "type": "string"},
    {"name": "email", "type": "string", "default": ""}  ← новое поле
  ]
}

Старый consumer прочитает новые данные (игнорирует email)
Новый consumer прочитает старые данные (email = default)
```

**Best practices:**
```
✅ Используйте для production (не только dev)
✅ Устанавливайте совместимость на topic-level
✅ Версионируйте схемы явно
✅ Тестируйте совместимость перед deploy
❌ Не меняйте compatibility mode часто
```

***

## Вопрос 35

**Что такое Avro и зачем он нужен?**

Avro — это бинарный формат сериализации данных от Apache, использующий JSON-схемы для определения структуры данных, который обеспечивает компактное представление (меньше размер чем JSON), быструю сериализацию/десериализацию и сильную типизацию с поддержкой schema evolution; в связке с Schema Registry позволяет эволюционировать данные с гарантиями совместимости между версиями, что критично для долгоживущих Kafka топиков и микросервисной архитектуры.

**Преимущества Avro:**
```
✅ Компактный формат (на 30-50% меньше JSON)
✅ Быстрая сериализация (binary)
✅ Schema evolution (backward/forward compatibility)
✅ Сильная типизация (compile-time проверки)
✅ Нет field names в данных (схема отдельно)
✅ Dynamic typing (schema в runtime)
✅ Code generation для Java/Python/etc
```

**Avro Schema пример:**
```json
{
  "type": "record",
  "name": "Order",
  "namespace": "com.example.avro",
  "fields": [
    {"name": "orderId", "type": "long"},
    {"name": "userId", "type": "long"},
    {"name": "amount", "type": "double"},
    {"name": "currency", "type": "string", "default": "USD"},
    {"name": "items", "type": {
      "type": "array",
      "items": {
        "type": "record",
        "name": "OrderItem",
        "fields": [
          {"name": "productId", "type": "long"},
          {"name": "quantity", "type": "int"}
        ]
      }
    }}
  ]
}
```

**Использование с Kafka:**
```java
// Maven: org.apache.avro:avro, io.confluent:kafka-avro-serializer

// Producer
props.put("value.serializer", "io.confluent.kafka.serializers.KafkaAvroSerializer");
props.put("schema.registry.url", "http://localhost:8081");

// Generated Avro class (от схемы)
Order order = Order.newBuilder()
    .setOrderId(123L)
    .setUserId(456L)
    .setAmount(99.99)
    .setCurrency("USD")
    .setItems(items)
    .build();

producer.send(new ProducerRecord<>("orders", order.getUserId().toString(), order));

// Consumer
props.put("value.deserializer", "io.confluent.kafka.serializers.KafkaAvroDeserializer");
props.put("specific.avro.reader", "true");

Order order = (Order) record.value();
```

**Schema Evolution:**
```
Version 1 → Version 2:
✅ Добавить поле с default
✅ Удалить поле с default
✅ Изменить default value
❌ Изменить тип поля
❌ Переименовать поле (используйте aliases)

Пример:
{"name": "status", "type": "string", "default": "PENDING"}
→ старые данные без "status" получат "PENDING"
```

**Когда использовать:**
```
✅ Production Kafka topics
✅ Долгоживущие данные
✅ Микросервисы (независимая эволюция)
✅ Event sourcing
✅ High throughput (компактность важна)

❌ Не нужен для:
- Простые string сообщения
- Debugging (JSON читаемее)
- Прототипирование
```

***

## Вопрос 36

**Как обеспечить порядок сообщений в Kafka?**

Порядок сообщений в Kafka гарантируется только внутри одной партиции, поэтому для обеспечения упорядоченности необходимо: использовать одинаковый ключ для связанных событий (hash(key) направляет их в одну партицию), настроить max.in.flight.requests.per.connection=1 или включить идемпотентность (enable.idempotence=true для упорядоченности с retry), и читать одним консюмером из партиции (или одной consumer group где партиция → один консюмер).

**Гарантии порядка:**
```
Внутри партиции:
✅ Записи упорядочены по offset
✅ Один продюсер → порядок сохранён
✅ Retry не нарушает порядок (с idempotence)

Между партициями:
❌ Нет гарантий порядка
❌ Разные партиции → параллельная обработка
```

**Producer конфигурация для порядка:**
```java
// Вариант 1: Idempotent Producer (рекомендуется)
props.put("enable.idempotence", "true");
// Автоматически устанавливает:
// max.in.flight.requests.per.connection=5
// retries=Integer.MAX_VALUE
// acks=all

// Вариант 2: Строгий порядок (медленнее)
props.put("max.in.flight.requests.per.connection", "1");
props.put("retries", 3);
props.put("acks", "all");
```

**Использование Key для порядка:**
```java
// Все заказы пользователя user-123 в одной партиции → порядок
producer.send(new ProducerRecord<>("orders", "user-123", order1));
producer.send(new ProducerRecord<>("orders", "user-123", order2));
producer.send(new ProducerRecord<>("orders", "user-123", order3));

// Разные пользователи могут быть в разных партициях (без порядка между ними)
producer.send(new ProducerRecord<>("orders", "user-456", order4));
```

**Consumer стратегия:**
```java
// Обработка в порядке внутри партиции
while (true) {
    records = consumer.poll(Duration.ofMillis(100));
    
    for (ConsumerRecord<String, String> record : records) {
        // Записи из одной партиции упорядочены
        processInOrder(record);
    }
    
    // Commit после обработки всех
    consumer.commitSync();
}
```

**Trade-offs:**
```
Строгий порядок (1 партиция):
✅ Полный порядок всех событий
❌ Нет параллелизма
❌ Throughput ограничен одной партицией
❌ SPOF для обработки

Порядок по ключу (N партиций):
✅ Порядок внутри entity (user, order)
✅ Параллелизм (разные entity → разные партиции)
✅ Высокий throughput
❌ Нет глобального порядка
```

**Best practices:**
```
✅ Используйте идемпотентность (enable.idempotence=true)
✅ Выбирайте правильный sharding key (user_id, order_id)
✅ Одна партиция для strict ordering (если throughput позволяет)
✅ Дизайн системы должен учитывать eventual ordering
❌ Не полагайтесь на порядок между партициями
```

***

## Вопрос 37

**Что такое Dead Letter Queue (DLQ)?**

Dead Letter Queue (DLQ) — это паттерн обработки ошибок в Kafka, где сообщения, которые не удалось обработать после нескольких попыток (например, из-за некорректных данных, бизнес-логических ошибок или downstream сервис недоступен), перенаправляются в отдельный топик для последующего анализа, ручной обработки или мониторинга, предотвращая блокировку основного потока обработки.

**Зачем нужен DLQ:**
```
Проблема без DLQ:
1. Сообщение с ошибкой
2. Консюмер пытается обработать → fail
3. Не commit offset
4. Poll снова → fail → бесконечный loop
5. Блокировка обработки всей партиции

С DLQ:
1. Сообщение с ошибкой
2. После N попыток → отправить в DLQ
3. Commit offset
4. Продолжить обработку следующих сообщений
```

**Реализация:**
```java
@Service
public class OrderConsumer {
    
    @Autowired
    private KafkaTemplate<String, String> kafkaTemplate;
    
    private static final int MAX_RETRIES = 3;
    
    @KafkaListener(topics = "orders")
    public void consume(ConsumerRecord<String, String> record) {
        try {
            processOrder(record.value());
        } catch (Exception e) {
            handleError(record, e);
        }
    }
    
    private void handleError(ConsumerRecord<String, String> record, Exception e) {
        // Проверка retry count (можно хранить в headers)
        int retryCount = getRetryCount(record);
        
        if (retryCount >= MAX_RETRIES) {
            // Отправка в DLQ
            ProducerRecord<String, String> dlqRecord = new ProducerRecord<>(
                "orders-dlq",
                record.key(),
                record.value()
            );
            
            // Добавление метаданных
            dlqRecord.headers()
                .add("original_topic", record.topic().getBytes())
                .add("original_partition", String.valueOf(record.partition()).getBytes())
                .add("original_offset", String.valueOf(record.offset()).getBytes())
                .add("error_message", e.getMessage().getBytes())
                .add("retry_count", String.valueOf(retryCount).getBytes());
            
            kafkaTemplate.send(dlqRecord);
            log.error("Sent to DLQ after {} retries: {}", retryCount, record.key());
        } else {
            // Retry (можно с backoff через headers)
            retryWithBackoff(record, retryCount + 1);
        }
    }
}
```

**Kafka Connect DLQ конфигурация:**
```properties
# Connector config
errors.tolerance=all
errors.deadletterqueue.topic.name=my-connector-dlq
errors.deadletterqueue.topic.replication.factor=3
errors.deadletterqueue.context.headers.enable=true
```

**Мониторинг DLQ:**
```java
@Service
public class DLQMonitor {
    
    @Scheduled(fixedRate = 60000)  // Каждую минуту
    public void checkDLQ() {
        ConsumerRecords<String, String> records = dlqConsumer.poll(Duration.ofSeconds(10));
        
        if (!records.isEmpty()) {
            log.warn("DLQ contains {} messages", records.count());
            
            // Alert (PagerDuty, Slack, etc)
            alertService.sendAlert("DLQ has messages requiring attention");
            
            // Optional: автоматический replay после fix
            for (ConsumerRecord<String, String> record : records) {
                analyzeAndRetry(record);
            }
        }
    }
}
```

**Best practices:**
```
✅ Всегда имейте DLQ для production
✅ Логируйте полный context (headers, timestamps)
✅ Мониторьте размер DLQ
✅ Периодически анализируйте причины ошибок
✅ Implement replay mechanism для исправленных данных
❌ Не игнорируйте DLQ
❌ Не используйте DLQ для business logic
```

***

## Вопрос 38

**Как масштабировать Kafka кластер?**

Масштабирование Kafka кластера включает horizontal scaling (добавление брокеров для увеличения throughput и storage capacity), увеличение партиций в топиках (для параллелизма продюсеров/консюмеров), partition reassignment (перебалансировка партиций между брокерами), и vertical scaling (улучшение железа брокеров для CPU/disk/network intensive задач); ключевые метрики для принятия решений — CPU utilization, disk I/O, network throughput, partition count per broker.

**Добавление брокеров:**
```bash
# 1. Настроить новый broker (server.properties)
broker.id=4  # Уникальный ID
log.dirs=/var/kafka-logs
zookeeper.connect=zk1:2181,zk2:2181,zk3:2181

# 2. Запустить новый broker
kafka-server-start.sh config/server.properties

# 3. Брокер автоматически join кластер
# Но партиции НЕ перемещаются автоматически!

# 4. Partition reassignment (см. вопрос 39)
```

**Увеличение партиций:**
```bash
# Можно только увеличивать (не уменьшать!)
kafka-topics.sh --alter \
  --topic orders \
  --partitions 12 \  # Было 6, стало 12
  --bootstrap-server localhost:9092

# Warning: изменяет партиционирование по ключам
# hash(key) % 6 != hash(key) % 12
# Сообщения с одним key пойдут в другую партицию
```

**Вертикальное масштабирование:**
```
CPU:
- Больше ядер для network/IO threads
- num.network.threads=8
- num.io.threads=16

Memory:
- Больше RAM для OS page cache
- Не увеличивайте heap (max 6GB)
- Остальное для page cache

Disk:
- SSD вместо HDD (10x faster)
- RAID10 для производительности
- Separate disks для data/logs

Network:
- 10Gbps+ для high-throughput
- Separate network для replication
```

**Capacity Planning:**
```
Формулы:

Brokers нужно:
= (Total throughput / Single broker throughput) * RF

Пример:
Target: 1GB/s write
Broker throughput: 200MB/s
Replication factor: 3
Brokers = (1000MB / 200MB) * 3 = 15 brokers

Partitions per broker:
Рекомендация: <4000 партиций на broker
(включая replicas)

Disk capacity:
= Throughput * Retention * Replication factor
= 100MB/s * 7 days * 3 = ~180TB
```

**Best practices:**
```
✅ Планируйте growth заранее
✅ Мониторьте ключевые метрики
✅ Используйте rack awareness
✅ Тестируйте reassignment на staging
✅ Graceful scaling (один broker за раз)
❌ Не добавляйте partition бездумно (hash change)
❌ Не превышайте 4000 partitions/broker
```

***

## Вопрос 39

**Что такое Partition Reassignment?**

Partition Reassignment — это процесс перемещения партиций и их реплик между брокерами кластера для балансировки нагрузки после добавления новых брокеров, decommission старых или при неравномерном распределении leader'ов; выполняется через kafka-reassign-partitions.sh инструмент, который генерирует план миграции, копирует данные и обновляет метаданные, при этом топик остаётся доступным, но нагрузка на кластер возрастает.

**Когда нужен reassignment:**
```
1. Новые брокеры добавлены (партиции не мигрируют автоматически)
2. Broker decommission (переместить партиции перед остановкой)
3. Disk imbalance (неравномерное использование дисков)
4. Leader imbalance (неравномерная нагрузка на брокеры)
5. Rack awareness (распределение по availability zones)
```

**Процесс reassignment:**
```bash
# 1. Создать JSON с топиками для reassignment
cat > topics-to-move.json <<EOF
{
  "topics": [
    {"topic": "orders"}
  ],
  "version": 1
}
EOF

# 2. Сгенерировать план reassignment
kafka-reassign-partitions.sh \
  --bootstrap-server localhost:9092 \
  --topics-to-move-json-file topics-to-move.json \
  --broker-list "0,1,2,3,4" \  # Новые broker IDs
  --generate

# Output:
# Current partition replica assignment (сохраните для rollback!)
# Proposed partition reassignment (используйте для execute)

# 3. Сохранить proposed plan
cat > reassignment-plan.json <<EOF
{
  "version":1,
  "partitions":[
    {"topic":"orders","partition":0,"replicas":[1,2,3],"log_dirs":["any","any","any"]},
    {"topic":"orders","partition":1,"replicas":[2,3,4],"log_dirs":["any","any","any"]}
  ]
}
EOF

# 4. Выполнить reassignment
kafka-reassign-partitions.sh \
  --bootstrap-server localhost:9092 \
  --reassignment-json-file reassignment-plan.json \
  --execute

# 5. Проверить прогресс
kafka-reassign-partitions.sh \
  --bootstrap-server localhost:9092 \
  --reassignment-json-file reassignment-plan.json \
  --verify

# 6. Throttle replication (опционально, для production)
kafka-reassign-partitions.sh \
  --bootstrap-server localhost:9092 \
  --reassignment-json-file reassignment-plan.json \
  --throttle 50000000 \  # 50MB/s
  --execute
```

**Preferred Leader Election:**
```bash
# Вернуть лидерство к preferred replicas (первая в списке)
kafka-leader-election.sh \
  --bootstrap-server localhost:9092 \
  --election-type PREFERRED \
  --all-topic-partitions
```

**Мониторинг reassignment:**
```
Метрики:
- ReassigningPartitions: количество партиций в процессе
- BytesInPerSec: увеличение из-за replication
- NetworkProcessorAvgIdlePercent: загрузка сети

Время:
Зависит от размера данных и throttle:
1TB данных / 50MB/s = ~5.5 часов
```

**Best practices:**
```
✅ Делайте reassignment в low-traffic периоды
✅ Используйте throttling в production
✅ Сохраняйте current assignment для rollback
✅ Мониторьте cluster health во время reassignment
✅ Тестируйте на staging первым
❌ Не reassign все партиции сразу
❌ Не забывайте про rack awareness
```

***

## Вопрос 40

**Как мониторить Kafka кластер?**

Мониторинг Kafka включает отслеживание JMX метрик брокеров (CPU, memory, disk I/O, network throughput, UnderReplicatedPartitions), consumer lag через kafka-consumer-groups.sh или Burrow, producer/consumer metrics (request latency, throughput, error rate), и cluster-level показателей (controller status, ISR shrinks, offline partitions); для визуализации используют Prometheus + Grafana, Confluent Control Center, или специализированные инструменты вроде Kafka Manager, Cruise Control.

**Ключевые метрики брокера (JMX):**
```
Critical (алерты обязательны):
- UnderReplicatedPartitions: партиции без достаточных ISR
  → должно быть 0
- OfflinePartitionsCount: недоступные партиции
  → должно быть 0
- ActiveControllerCount: количество контроллеров
  → должно быть 1 (ровно один)
- ISRShrinkRate / ISRExpandRate: частота изменений ISR
  → высокая → проблемы с реплицацией

Performance:
- BytesInPerSec / BytesOutPerSec: throughput
- MessagesInPerSec: rate сообщений
- RequestHandlerAvgIdlePercent: загрузка request threads
  → <20% = перегрузка
- NetworkProcessorAvgIdlePercent: загрузка network threads
- LeaderElectionRateAndTimeMs: частота выборов лидеров
  → высокая = нестабильность
```

**Producer metrics:**
```java
// Через JMX или Producer metrics()
Map<MetricName, ? extends Metric> metrics = producer.metrics();

Ключевые:
- record-send-rate: сообщений в секунду
- record-error-rate: ошибок в секунду
- request-latency-avg: средняя latency
- request-latency-max: максимальная latency
- buffer-available-bytes: свободная память buffer
- batch-size-avg: средний размер batch
```

**Consumer lag мониторинг:**
```bash
# CLI
kafka-consumer-groups.sh --bootstrap-server localhost:9092 \
  --group my-group --describe

# Burrow (LinkedIn tool)
# REST API для consumer lag
curl http://burrow:8000/v3/kafka/local/consumer/my-group/lag

# Output JSON с lag per partition
```

**Prometheus + Grafana setup:**
```yaml
# docker-compose.yml для мониторинга
version: '3'
services:
  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
  
  jmx-exporter:
    image: sscaling/jmx-prometheus-exporter
    ports:
      - "5556:5556"
    environment:
      - SERVICE_PORT=5556
    volumes:
      - ./kafka-jmx-config.yml:/etc/jmx-exporter/config.yml
  
  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
```

**Alerting rules (Prometheus):**
```yaml
groups:
- name: kafka_alerts
  rules:
  - alert: UnderReplicatedPartitions
    expr: kafka_server_replicamanager_underreplicatedpartitions > 0
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "Kafka has under-replicated partitions"
  
  - alert: OfflinePartitions
    expr: kafka_controller_kafkacontroller_offlinepartitionscount > 0
    for: 1m
    labels:
      severity: critical
  
  - alert: HighConsumerLag
    expr: kafka_consumer_group_lag > 10000
    for: 10m
    labels:
      severity: warning
```

**Инструменты мониторинга:**
```
Open Source:
- Kafka Manager (CMAK): UI для cluster management
- Cruise Control: автоматическая балансировка
- Burrow: consumer lag monitoring
- Prometheus + Grafana: метрики и дашборды

Commercial:
- Confluent Control Center: полный мониторинг стэк
- Datadog Kafka integration
- New Relic Kafka monitoring
```

**Best practices:**
```
✅ Мониторьте все три уровня: broker, producer, consumer
✅ Настройте алерты на critical метрики
✅ Храните метрики долгосрочно (capacity planning)
✅ Дашборды для каждой команды
✅ Мониторьте consumer lag активно
❌ Не игнорируйте warning алерты
❌ Не полагайтесь только на broker метрики
```

***

## Вопрос 41

**Что такое Kafka Streams?**

Kafka Streams — это клиентская библиотека Java/Scala для построения event-driven приложений и microservices, которая читает данные из Kafka топиков, выполняет stateful или stateless трансформации (фильтрация, mapping, aggregation, joins, windowing) и записывает результаты обратно в Kafka; работает как обычное приложение без внешних зависимостей (не требует отдельного кластера типа Spark/Flink), автоматически масштабируется через партиции и поддерживает exactly-once семантику.

**Ключевые особенности:**
```
✅ Обычная Java библиотека (не фреймворк)
✅ Не требует отдельного кластера обработки
✅ Масштабирование через Kafka партиции
✅ Stateful processing (state stores, joins, aggregations)
✅ Exactly-once processing (transactional)
✅ Event-time processing с windowing
✅ Fault tolerance через Kafka (replay, state restore)
✅ Low latency (миллисекунды)
```

**Простой пример:**
```java
Properties props = new Properties();
props.put(StreamsConfig.APPLICATION_ID_CONFIG, "word-count-app");
props.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");

StreamsBuilder builder = new StreamsBuilder();

// Stream из топика
KStream<String, String> textLines = builder.stream("input-topic");

// Обработка: split, map, groupBy, count
KTable<String, Long> wordCounts = textLines
    .flatMapValues(line -> Arrays.asList(line.toLowerCase().split("\\W+")))
    .groupBy((key, word) -> word)
    .count();

// Запись результата
wordCounts.toStream().to("output-topic");

// Запуск
KafkaStreams streams = new KafkaStreams(builder.build(), props);
streams.start();
```

**Сравнение с другими фреймворками:**

| Аспект | Kafka Streams | Spark Streaming | Flink |
|--------|--------------|-----------------|-------|
| Deployment | Приложение | Кластер | Кластер |
| Latency | Миллисекунды | Секунды (micro-batch) | Миллисекунды |
| State | Local state stores | In-memory/external | In-memory |
| Scaling | Kafka partitions | Executors | Task slots |
| Complexity | Низкая | Средняя | Высокая |

**Когда использовать:**
```
✅ Stream processing на Kafka данных
✅ Microservices с event-driven логикой
✅ Real-time analytics
✅ Data enrichment, filtering
✅ Change Data Capture (CDC) обработка
✅ Event sourcing patterns

❌ Не нужен для:
- Batch processing (используйте Spark)
- ML training (Spark/Flink)
- Сложные joins с внешними БД
- Требуется SQL interface
```

***

## Вопрос 42

**Что такое KStream и KTable?**

KStream и KTable — это две основные абстракции в Kafka Streams, где KStream представляет unbounded stream событий (append-only последовательность, каждая запись — отдельное событие), а KTable представляет changelog stream (материализованное представление последнего состояния для каждого ключа, аналог таблицы БД с update/delete семантикой); выбор между ними зависит от того, нужна ли история событий или только актуальное состояние.

**KStream (Event Stream):**
```
Характеристики:
- Каждая запись — независимое событие
- Append-only (immutable)
- All records важны
- Похоже на INSERT в БД

Пример: лог событий
key=user-1, value="login"     (событие 1)
key=user-1, value="view_page" (событие 2)
key=user-1, value="logout"    (событие 3)

Все три записи сохраняются и обрабатываются
```

**KTable (Changelog/State):**
```
Характеристики:
- Последнее состояние для ключа
- Updates/Deletes (mutable view)
- Only latest value важен
- Похоже на UPDATE в БД

Пример: user profile
key=user-1, value={name:"Alice", age:25}
key=user-1, value={name:"Alice", age:26}  ← обновление
→ KTable хранит только последнее

Compacted topic в Kafka
```

**Код примеры:**
```java
StreamsBuilder builder = new StreamsBuilder();

// KStream: все события
KStream<String, String> events = builder.stream("user-events");
events.foreach((key, value) -> 
    System.out.println("Event: " + key + " -> " + value)
);

// KTable: последнее состояние
KTable<String, String> users = builder.table("user-profiles");
users.toStream().foreach((key, value) -> 
    System.out.println("Current state: " + key + " -> " + value)
);

// Из KStream в KTable (aggregation)
KTable<String, Long> counts = events
    .groupByKey()
    .count();

// Из KTable в KStream
KStream<String, String> userStream = users.toStream();
```

**Операции:**
```java
// KStream операции (stateless)
stream
    .filter((key, value) -> value.length() > 5)
    .map((key, value) -> KeyValue.pair(key, value.toUpperCase()))
    .flatMap((key, value) -> Arrays.asList(value.split(",")))
    .foreach((key, value) -> log(key, value));

// KTable операции (stateful)
table
    .filter((key, value) -> value != null)
    .mapValues(value -> value.toUpperCase())
    .toStream()  // Преобразование в KStream
    .to("output-topic");

// Aggregation (KStream → KTable)
KTable<String, Long> aggregated = stream
    .groupByKey()
    .aggregate(
        () -> 0L,  // initializer
        (key, value, agg) -> agg + 1  // aggregator
    );
```

**Выбор между KStream и KTable:**
```
Используйте KStream:
✅ Лог событий (clicks, purchases, logins)
✅ Audit trail
✅ Time-series data
✅ Каждое событие имеет значение

Используйте KTable:
✅ User profile
✅ Product catalog
✅ Configuration
✅ Inventory (latest quantity)
✅ Нужно только текущее состояние
```

**Join'ы:**
```java
// KStream-KStream join (событие-событие)
KStream<String, String> joined = stream1.join(
    stream2,
    (value1, value2) -> value1 + value2,
    JoinWindows.of(Duration.ofMinutes(5))
);

// KStream-KTable join (обогащение)
KStream<String, String> enriched = stream.join(
    table,
    (streamValue, tableValue) -> streamValue + tableValue
);

// KTable-KTable join (состояние-состояние)
KTable<String, String> joined = table1.join(
    table2,
    (value1, value2) -> value1 + value2
);
```

***

## Вопрос 43

**Что такое GlobalKTable?**

GlobalKTable — это специальный тип KTable в Kafka Streams, который полностью реплицируется на каждый instance приложения (в отличие от обычной KTable, партиции которой распределены между instances), что позволяет выполнять joins с KStream без co-partitioning requirement и обеспечивает локальный доступ ко всем данным таблицы, но требует больше памяти и подходит только для относительно небольших reference данных.

**Отличия от KTable:**
```
KTable:
- Партиционирована (каждый instance хранит subset)
- Требует co-partitioning для join
- Масштабируется горизонтально
- Подходит для больших datasets

GlobalKTable:
- Полностью реплицирована на каждый instance
- Не требует co-partitioning
- Все данные локально на каждом instance
- Подходит только для small datasets (<GB)
```

**Создание:**
```java
StreamsBuilder builder = new StreamsBuilder();

// Обычная KTable (партиционирована)
KTable<String, String> regularTable = builder.table("users");

// GlobalKTable (полная копия на каждом instance)
GlobalKTable<String, String> globalTable = builder.globalTable("products");
```

**Join без co-partitioning:**
```java
// KStream-GlobalKTable join
// Не требует одинакового количества партиций или ключей!

KStream<String, Order> orders = builder.stream("orders");
GlobalKTable<String, Product> products = builder.globalTable("products");

// Join используя productId из order (не key)
KStream<String, OrderWithProduct> enriched = orders.join(
    products,
    (orderId, order) -> order.getProductId(),  // KeyValueMapper для GlobalKTable
    (order, product) -> new OrderWithProduct(order, product)
);
```

**Сравнение join'ов:**
```java
// KStream-KTable join (требует co-partitioning)
KStream<String, String> stream = builder.stream("stream-topic");  // 10 партиций
KTable<String, String> table = builder.table("table-topic");      // 10 партиций
// Key должен совпадать!
stream.join(table, (streamVal, tableVal) -> streamVal + tableVal);

// KStream-GlobalKTable join (БЕЗ co-partitioning)
GlobalKTable<String, String> globalTable = builder.globalTable("global-topic");
// Партиции могут быть разные, ключ не обязан совпадать
stream.join(
    globalTable,
    (key, value) -> extractJoinKey(value),  // Динамический ключ
    (streamVal, globalVal) -> streamVal + globalVal
);
```

**Trade-offs:**
```
GlobalKTable плюсы:
✅ Гибкие join'ы (любой ключ)
✅ Быстрые локальные lookups
✅ Не нужен co-partitioning
✅ Foreign-key joins

GlobalKTable минусы:
❌ Полная репликация (N instances = N copies)
❌ Больше memory usage
❌ Медленнее startup (загрузка всех данных)
❌ Не масштабируется для больших данных

Используйте GlobalKTable для:
- Reference data (countries, products, configurations)
- Lookup tables (<1GB)
- Foreign-key enrichment
```

**Пример reference data:**
```java
// products — маленькая таблица (~1000 записей)
GlobalKTable<String, Product> products = builder.globalTable("products");

// orders — большой stream
KStream<String, Order> orders = builder.stream("orders");

// Обогащение заказов данными продукта
KStream<String, EnrichedOrder> enrichedOrders = orders.join(
    products,
    (orderId, order) -> order.getProductId(),
    (order, product) -> {
        return new EnrichedOrder(
            order,
            product.getName(),
            product.getPrice(),
            product.getCategory()
        );
    }
);
```

***

## Вопрос 44

**Как работают Join операции в Kafka Streams?**

Join операции в Kafka Streams объединяют данные из двух топиков на основе ключей, при этом существуют разные типы joins (inner, left, outer) для KStream-KStream (windowed join событий в пределах временного окна), KStream-KTable/GlobalKTable (обогащение событий актуальным состоянием), и KTable-KTable (join двух changelog streams); для KStream-KStream и KStream-KTable требуется co-partitioning (одинаковое количество партиций и партиционирование по ключу), кроме GlobalKTable.

**Типы join'ов:**

**1. KStream-KStream Join (Event-Event):**
```java
// Windowed join: события должны попасть в одно временное окно
KStream<String, String> stream1 = builder.stream("clicks");
KStream<String, String> stream2 = builder.stream("impressions");

// Inner join (оба события должны быть)
KStream<String, String> innerJoined = stream1.join(
    stream2,
    (click, impression) -> "Click: " + click + ", Impression: " + impression,
    JoinWindows.of(Duration.ofMinutes(5))  // Окно 5 минут
);

// Left join (левое обязательно, правое опционально)
KStream<String, String> leftJoined = stream1.leftJoin(
    stream2,
    (click, impression) -> "Click: " + click + ", Impression: " + impression,
    JoinWindows.of(Duration.ofMinutes(5))
);

// Outer join (хотя бы одно из событий)
KStream<String, String> outerJoined = stream1.outerJoin(
    stream2,
    (click, impression) -> "Click: " + click + ", Impression: " + impression,
    JoinWindows.of(Duration.ofMinutes(5))
);
```

**2. KStream-KTable Join (Event-State):**
```java
// Обогащение события актуальным состоянием
KStream<String, Order> orders = builder.stream("orders");
KTable<String, Customer> customers = builder.table("customers");

// Inner join
KStream<String, OrderWithCustomer> enriched = orders.join(
    customers,
    (order, customer) -> new OrderWithCustomer(order, customer)
);

// Left join (customer может не существовать)
KStream<String, OrderWithCustomer> leftEnriched = orders.leftJoin(
    customers,
    (order, customer) -> new OrderWithCustomer(order, customer)
);
```

**3. KStream-GlobalKTable Join:**
```java
GlobalKTable<String, Product> products = builder.globalTable("products");
KStream<String, Order> orders = builder.stream("orders");

// Гибкий join по любому ключу
KStream<String, OrderWithProduct> enriched = orders.join(
    products,
    (orderId, order) -> order.getProductId(),  // Динамический ключ
    (order, product) -> new OrderWithProduct(order, product)
);
```

**4. KTable-KTable Join:**
```java
// Join двух changelog streams
KTable<String, Customer> customers = builder.table("customers");
KTable<String, Address> addresses = builder.table("addresses");

// Inner join
KTable<String, CustomerWithAddress> joined = customers.join(
    addresses,
    (customer, address) -> new CustomerWithAddress(customer, address)
);

// Left/Outer joins тоже поддерживаются
```

**Co-partitioning requirement:**
```
Для KStream-KStream и KStream-KTable:

✅ Требования:
1. Одинаковое количество партиций
2. Одинаковая партиционирование стратегия (hash)
3. Одинаковые ключи для join

❌ Если нарушено:
TopologyException: can't join because of different number of partitions

Решение:
1. Repartition через through()
2. Использовать GlobalKTable
```

**Repartitioning пример:**
```java
// Топики с разным количеством партиций
KStream<String, Order> orders = builder.stream("orders");  // 6 партиций
KTable<String, User> users = builder.table("users");       // 10 партиций

// Repartition orders для co-partitioning
KStream<String, Order> repartitioned = orders
    .selectKey((key, order) -> order.getUserId())  // Изменить ключ
    .through("orders-repartitioned",               // Промежуточный топик
        Produced.with(Serdes.String(), orderSerde)
            .withStreamPartitioner((topic, key, value, numPartitions) -> 
                Math.abs(key.hashCode()) % numPartitions));

// Теперь join возможен
KStream<String, OrderWithUser> joined = repartitioned.join(
    users,
    (order, user) -> new OrderWithUser(order, user)
);
```

**Windowing в joins:**
```java
// Временное окно для KStream-KStream join
JoinWindows window = JoinWindows.of(Duration.ofMinutes(5))
    .after(Duration.ofMinutes(2))    // Искать 2 мин вперёд
    .before(Duration.ofMinutes(3))   // Искать 3 мин назад
    .grace(Duration.ofMinutes(1));   // Grace period для поздних событий

KStream<String, String> joined = stream1.join(
    stream2,
    (value1, value2) -> value1 + value2,
    window
);
```

**Performance considerations:**
```
- KStream-KStream join требует state store (disk I/O)
- Windowed joins хранят данные в пределах окна
- GlobalKTable join быстрее (локальный lookup)
- Большие окна = больше state store
```

***

## Вопрос 45

**Что такое Windowing в Kafka Streams?**

Windowing в Kafka Streams — это механизм группировки событий в конечные временные интервалы (окна) для выполнения агрегаций, joins или stateful операций на потоке данных; существуют типы окон: Tumbling (фиксированные неперекрывающиеся), Hopping (фиксированные перекрывающиеся), Sliding (динамические по времени между событиями), Session (динамические по inactivity gap); поддерживается event-time processing с grace period для обработки поздних событий.

**Типы окон:**

**1. Tumbling Windows (Fixed, Non-Overlapping):**
```java
// Фиксированные неперекрывающиеся окна
Duration windowSize = Duration.ofMinutes(5);

KTable<Windowed<String>, Long> counts = stream
    .groupByKey()
    .windowedBy(TimeWindows.of(windowSize))
    .count();

// Пример:
// 00:00-00:05 → window 1
// 00:05-00:10 → window 2
// 00:10-00:15 → window 3
// Событие попадает ровно в одно окно
```

**2. Hopping Windows (Fixed, Overlapping):**
```java
// Перекрывающиеся окна
Duration windowSize = Duration.ofMinutes(5);
Duration advance = Duration.ofMinutes(2);

KTable<Windowed<String>, Long> counts = stream
    .groupByKey()
    .windowedBy(TimeWindows.of(windowSize).advanceBy(advance))
    .count();

// Пример:
// 00:00-00:05 → window 1
// 00:02-00:07 → window 2
// 00:04-00:09 → window 3
// Событие может попасть в несколько окон
```

**3. Sliding Windows:**
```java
// Окно определяется разницей времени между событиями
Duration timeDifference = Duration.ofMinutes(5);

KTable<Windowed<String>, Long> counts = stream
    .groupByKey()
    .windowedBy(SlidingWindows.withTimeDifferenceAndGrace(
        timeDifference,
        Duration.ofMinutes(1)  // grace period
    ))
    .count();

// Используется для joins: события в пределах 5 минут друг от друга
```

**4. Session Windows:**
```java
// Динамические окна на основе inactivity gap
Duration inactivityGap = Duration.ofMinutes(5);

KTable<Windowed<String>, Long> counts = stream
    .groupByKey()
    .windowedBy(SessionWindows.with(inactivityGap))
    .count();

// Пример:
// Event 1 at 00:00
// Event 2 at 00:03 → в том же окне (gap < 5 мин)
// Event 3 at 00:12 → новое окно (gap > 5 мин)
```

**Event-time vs Processing-time:**
```java
// Event-time (используется timestamp из записи)
stream
    .groupByKey()
    .windowedBy(TimeWindows.of(Duration.ofMinutes(5)))
    .count();

// Извлечение timestamp из payload (если нужно)
stream.transformValues(() -> new ValueTransformer<String, String>() {
    @Override
    public String transform(String value) {
        long eventTime = extractTimestamp(value);
        context().forward(value, To.all().withTimestamp(eventTime));
        return null;
    }
});
```

**Grace Period (поздние события):**
```java
// Grace period: дополнительное время ожидания поздних событий
TimeWindows window = TimeWindows.of(Duration.ofMinutes(5))
    .grace(Duration.ofMinutes(1));  // Ждём 1 минуту после закрытия окна

// Пример:
// Окно: 00:00-00:05
// Закрывается: 00:05
// Grace period: до 00:06
// События с timestamp 00:04, прибывшие до 00:06 → обработаются
// После 00:06 → игнорируются или в late-arriving topic
```

**Aggregation в окнах:**
```java
// Count в окне
KTable<Windowed<String>, Long> wordCounts = textLines
    .flatMapValues(line -> Arrays.asList(line.split(" ")))
    .groupBy((key, word) -> word)
    .windowedBy(TimeWindows.of(Duration.ofMinutes(5)))
    .count();

// Aggregate с custom logic
KTable<Windowed<String>, Double> averages = stream
    .groupByKey()
    .windowedBy(TimeWindows.of(Duration.ofMinutes(5)))
    .aggregate(
        () -> new AggregateValue(0.0, 0),  // initializer
        (key, value, agg) -> {              // aggregator
            agg.sum += value;
            agg.count++;
            return agg;
        },
        Materialized.with(Serdes.String(), aggSerde)
    );
```

**Use cases:**
```
Tumbling:
- Метрики каждые 5 минут
- Hourly/daily aggregations
- Non-overlapping stats

Hopping:
- Moving averages
- Overlapping dashboards
- Smoother transitions

Session:
- User session analytics
- Click streams
- Activity tracking

Sliding:
- Joins с временными constraints
- Correlation analysis
```

***

## Вопрос 46

**Что такое State Store?**

State Store в Kafka Streams — это локальное хранилище (обычно RocksDB embedded database) на каждом instance приложения для сохранения промежуточных состояний stateful операций (aggregations, joins, windowing), которое автоматически управляется фреймворком, backup'ируется в changelog топик Kafka для fault tolerance и восстанавливается при перезапуске instance через replay changelog'а.

**Типы State Stores:**
```
1. Key-Value Store (default):
   - Простой map: key → value
   - Используется для aggregations, joins
   - Backed by RocksDB

2. Window Store:
   - Key → timestamp → value
   - Для windowed aggregations
   - Автоматическое retention старых окон

3. Session Store:
   - Key → session window → value
   - Для session windows
   - Динамическое управление окнами

4. Custom Store:
   - Пользовательская реализация
   - Для специфичных требований
```

**Автоматические state stores:**
```java
// State store создаётся автоматически для stateful операций
KTable<String, Long> counts = stream
    .groupByKey()
    .count();  // Автоматически создаёт key-value state store

// Windowed aggregation → window state store
KTable<Windowed<String>, Long> windowedCounts = stream
    .groupByKey()
    .windowedBy(TimeWindows.of(Duration.ofMinutes(5)))
    .count();
```

**Custom state store:**
```java
// Создание custom state store
StoreBuilder<KeyValueStore<String, Long>> storeBuilder = 
    Stores.keyValueStoreBuilder(
        Stores.persistentKeyValueStore("my-store"),
        Serdes.String(),
        Serdes.Long()
    );

// Регистрация в топологии
builder.addStateStore(storeBuilder);

// Использование в Transformer
stream.transform(() -> new Transformer<String, String, KeyValue<String, Long>>() {
    private KeyValueStore<String, Long> store;
    
    @Override
    public void init(ProcessorContext context) {
        this.store = (KeyValueStore<String, Long>) 
            context.getStateStore("my-store");
    }
    
    @Override
    public KeyValue<String, Long> transform(String key, String value) {
        Long count = store.get(key);
        count = (count == null) ? 1L : count + 1;
        store.put(key, count);
        return KeyValue.pair(key, count);
    }
}, "my-store");
```

**Changelog Topic (backup):**
```
State store автоматически backup в Kafka топик:

Топология:
Application ID: word-count-app
State store: counts-store

Changelog topic:
word-count-app-counts-store-changelog

Формат:
Key → State store key
Value → State store value
Compacted (cleanup.policy=compact)

При crash:
1. Новый instance назначается на партицию
2. Восстановление state из changelog
3. Resume processing
```

**Конфигурация:**
```java
// Настройка state store
Materialized<String, Long, KeyValueStore<Bytes, byte[]>> materialized = 
    Materialized.<String, Long, KeyValueStore<Bytes, byte[]>>as("my-store")
        .withKeySerde(Serdes.String())
        .withValueSerde(Serdes.Long())
        .withCachingEnabled()  // Включить caching
        .withLoggingEnabled(Collections.singletonMap(
            "retention.ms", "86400000"  // 1 день retention для changelog
        ));

KTable<String, Long> counts = stream
    .groupByKey()
    .count(materialized);
```

**In-Memory vs Persistent:**
```java
// In-memory store (быстрее, но не persisted)
Stores.inMemoryKeyValueStore("my-store");

// Persistent store (RocksDB, survived restart)
Stores.persistentKeyValueStore("my-store");
```

**State Store Directory:**
```properties
# Location на диске
state.dir=/tmp/kafka-streams

# Структура:
# /tmp/kafka-streams/
#   /{application.id}/
#     /{task-id}/
#       /rocksdb/
#         /{store-name}/
```

**Performance tuning:**
```properties
# RocksDB настройки
rocksdb.config.setter=com.example.MyRocksDBConfigSetter

# Cache size (default 10MB)
cache.max.bytes.buffering=10485760

# Commit interval
commit.interval.ms=30000
```

**Best practices:**
```
✅ Используйте автоматические stores где возможно
✅ Persistent stores для production
✅ Мониторьте размер state stores
✅ Настройте changelog retention
✅ SSD для лучшей производительности
❌ Не храните большие objects в state
❌ Не игнорируйте state store размер (disk full!)
```

***

Что такое Topology в Kafka Streams?

Topology в Kafka Streams — это направленный ациклический граф (DAG), описывающий последовательность преобразований, агрегирований, join'ов и других операций, которые над потоком данных выполняет приложение. Каждый узел топологии — это этап обработки (source, processor, stateful operator, sink), а рёбра отображают передачу данных. Топология определяет, каким образом данные перемещаются от входных топиков до выходных топиков (или materialized views).

Особенности:

Топология строится через StreamsBuilder и описывает, как данные текут и трансформируются.

Каждый state store, aggregations, joins или windowed операции в топологии — самостоятельный узел.

Один и тот же топик может быть источником (source node) для нескольких топологий одновременно; Kafka Streams управляет task partition assignment на уровне каждой топологии.

Для отладки доступен метод Topology.describe(), который выводит структуру DAG.

Complex topologies могут включать разветвление (branch), слияния (merge), вложенные sub-topologies и т.д.

Пример:

java
StreamsBuilder builder = new StreamsBuilder();
KStream<String, String> stream = builder.stream("input");

KTable<String, Long> counts = stream
    .flatMapValues(value -> Arrays.asList(value.split(" ")))
    .groupBy((key, word) -> word)
    .count();

counts.toStream().to("output");
Topology topology = builder.build();
System.out.println(topology.describe());
Вопрос 48
Как работает exactly-once в Kafka Streams?

Exactly-once в Kafka Streams — это семантика обработки, гарантирующая, что каждое событие будет обработано ровно один раз, даже при failover, сбоях и повторных запусках. Она реализована с помощью transactional producer (atomic commit данных и consumer offset в конце каждого processing task), встроенного в Kafka Streams, а также хранения состояния (state stores) на диске с changelog backup.

Ключевые элементы:

processing.guarantee=exactly_once_v2 (или просто exactly_once в старых версиях).

Каждый поток/partition обрабатывается task-ом, который использует транзакционный продюсер для отправки результатов и коммита consumer offset'ов в одной транзакции.

All stateful operators (agg, joins, windows) используют state stores c changelog, которые также коммитятся внутри транзакции.

Возможны transient дубли в output топике на время сбоя, но после восстановления гарантируется, что downstream consumers не увидят дублей.

При рестарте/ребалансе task восстанавливает состояние из changelog, а offsets — из transactional log.

Конфигурация:

java
props.put(StreamsConfig.PROCESSING_GUARANTEE_CONFIG, StreamsConfig.EXACTLY_ONCE_V2);
props.put(StreamsConfig.REPLICATION_FACTOR_CONFIG, 3);
props.put(ProducerConfig.ACKS_CONFIG, "all");
Trade-offs:

Выше latency, снижение throughput (в сравнении с at_least_once), особенно на write-heavy pipeline.

Требует правильно настроенных топиков (min.insync.replicas ≥ 2).

Вопрос 49
Что такое Interactive Queries?

Interactive Queries — это возможность Kafka Streams expose локальные state stores (KTable, windowed store, session store) через API для внешнего получения актуальных промежуточных или агрегированных данных (например, через REST endpoint), что позволяет строить быстро отвечающие на запросы приложения без дополнительных внешних БД.

Принцип работы:

Каждый instance Kafka Streams хранит shard состояния (partition state store).

Через Streams API пользователь может query локальный хранилище (например, store.get(key)).

Для распределённого запроса нужно определить, на каком instance находится нужный shard; Streams API предоставляет методы для поиска ("host for key", metadata).

Обычно поверх этого строят REST API, который redirect/проксирует запросы, если нужный store не на local instance.

Пример использования:

java
ReadOnlyKeyValueStore<String, Long> store = 
    streams.store(StoreQueryParameters.fromNameAndType(
      "counts-store", QueryableStoreTypes.keyValueStore()));

Long result = store.get("my-key");
Особенности:

Для агрегатов (KTable, windows) в low-latency аналитике.

Не подходит для API с жёсткими SLA на итоговую консистентность (state store обновляется асинхронно via changelog replay).

REST proxy слой зависит от корректной интеграции с Streams Metadata API.

Вопрос 50
Какие Best Practices для Kafka в production?

1. Топологии и партиции:

Планируйте необходимое количество партиций заранее (масштабируемость через consumers ≈ partitions).

Не превышайте 4–5k партиций на брокер.

Используйте полезные naming conventions для топиков.

2. Надёжность и безопасность:

Установите min.insync.replicas ≥ 2 и acks=all для важных данных (durability).

Включайте идемпотентность и транзакции для critical path операций (exactly-once).

Конфигурируйте ACLs и включайте TLS/SSL для трафика (особенно между датацентрами).

3. Продюсеры/консюмеры:

Контролируйте, когда и как коммитите offset'ы, чтобы не терять/не дублировать сообщения.

Мониторьте consumer lag и реагируйте на его рост.

Оптимизируйте параметры батчирования (linger.ms, batch.size, buffer.memory — баланс latency / throughput).

4. Данные и мониторинг:

Используйте Schema Registry и форматы с поддержкой schema evolution (Avro/Protobuf).

Мониторьте broker-level и client-side метрики (Throughput, Latency, UnderReplicatedPartitions, ActiveController, Lag).

Используйте инфраструктуру для алертинга (Prometheus+Grafana, Burrow).

5. Операционка:

Проводите periodic reassignment партиций для балансировки нагрузки.

Используйте rack awareness и SSD для кластеров с высокой пропускной способностью.

Планируйте retention в зависимости от бизнес-процессов и стоимости хранения (log cleaning vs log compaction для соответствующих use cases).

6. Streams:

Для stateful streaming workloads резервируйте достаточно disk/memory для state stores, храните state.dir на fast SSD.

Применяйте processing.guarantee=exactly_once_v2 или at_least_once согласно требованиям.

Делайте регулярные тесты на failover (перезагрузки, сетевые потери) — стриминг- и потоковые приложения должны быть fault-tolerant by design.

# 65 вопросов и ответов по System Design для Senior Java-разработчиков

## Содержание

### Основы System Design (вопросы 1-15)
1. [Что такое System Design?](#вопрос-1)
2. [High-Level Design vs Low-Level Design](#вопрос-2)
3. [Functional vs Non-Functional Requirements](#вопрос-3)
4. [Scalability — что это?](#вопрос-4)
5. [Vertical Scaling vs Horizontal Scaling](#вопрос-5)
6. [CAP Theorem](#вопрос-6)
7. [ACID vs BASE](#вопрос-7)
8. [Latency vs Throughput](#вопрос-8)
9. [Consistency Models](#вопрос-9)
10. [Availability и High Availability](#вопрос-10)
11. [Single Point of Failure (SPOF)](#вопрос-11)
12. [Fault Tolerance и Resilience](#вопрос-12)
13. [Load Balancer — зачем нужен?](#вопрос-13)
14. [Reverse Proxy vs Forward Proxy](#вопрос-14)
15. [API Gateway](#вопрос-15)

### Архитектурные паттерны (вопросы 16-30)
16. [Monolithic Architecture](#вопрос-16)
17. [Microservices Architecture](#вопрос-17)
18. [Service-Oriented Architecture (SOA)](#вопрос-18)
19. [Event-Driven Architecture](#вопрос-19)
20. [CQRS Pattern](#вопрос-20)
21. [Saga Pattern](#вопрос-21)
22. [Circuit Breaker Pattern](#вопрос-22)
23. [Strangler Fig Pattern](#вопрос-23)
24. [Sidecar Pattern](#вопрос-24)
25. [Backend for Frontend (BFF)](#вопрос-25)
26. [API Gateway Pattern](#вопрос-26)
27. [Service Mesh](#вопрос-27)
28. [Database per Service](#вопрос-28)
29. [Shared Database](#вопрос-29)
30. [Two-Phase Commit vs Saga](#вопрос-30)

### Базы данных и масштабирование (вопросы 31-45)
31. [SQL vs NoSQL](#вопрос-31)
32. [Database Indexing](#вопрос-32)
33. [Database Sharding](#вопрос-33)
34. [Database Partitioning](#вопрос-34)
35. [Database Replication](#вопрос-35)
36. [Master-Slave Replication](#вопрос-36)
37. [Master-Master Replication](#вопрос-37)
38. [Denormalization](#вопрос-38)
39. [Connection Pooling](#вопрос-39)
40. [N+1 Query Problem](#вопрос-40)
41. [Database Read Replicas](#вопрос-41)
42. [Write-Heavy vs Read-Heavy Systems](#вопрос-42)
43. [Eventually Consistent Systems](#вопрос-43)
44. [Distributed Transactions](#вопрос-44)
45. [Database Migration Strategies](#вопрос-45)

### Кэширование и производительность (вопросы 46-60)
46. [Caching Strategies](#вопрос-46)
47. [Cache-Aside (Lazy Loading)](#вопрос-47)
48. [Write-Through Cache](#вопрос-48)
49. [Write-Behind Cache](#вопрос-49)
50. [Cache Invalidation](#вопрос-50)
51. [Redis vs Memcached](#вопрос-51)
52. [CDN (Content Delivery Network)](#вопрос-52)
53. [Cache Eviction Policies](#вопрос-53)
54. [Cache Stampede Problem](#вопрос-54)
55. [HTTP Caching](#вопрос-55)
56. [Load Balancing Algorithms](#вопрос-56)
57. [Rate Limiting](#вопрос-57)
58. [Throttling vs Rate Limiting](#вопрос-58)
59. [Message Queues](#вопрос-59)
60. [Asynchronous Processing](#вопрос-60)

### Практические системы и best practices (вопросы 61-65)
61. [Design URL Shortener](#вопрос-61)
62. [Design Social Media Feed](#вопрос-62)
63. [Design Chat Application](#вопрос-63)
64. [Monitoring и Observability](#вопрос-64)
65. [System Design Interview Process](#вопрос-65)

---

## Вопрос 1

**Что такое System Design?**

System Design — это процесс определения архитектуры, компонентов, интерфейсов и данных для системы, удовлетворяющей specified requirements; включает принятие решений о технологиях, паттернах, trade-offs между consistency/availability/performance, и обеспечивает scalability, reliability, maintainability системы; на интервью оценивает способность кандидата проектировать large-scale distributed systems с учётом real-world constraints (budget, time, resources) и требований бизнеса.

**Ключевые аспекты:**
- **Requirements gathering**: понимание functional и non-functional требований
- **High-level design**: определение major компонентов и их взаимодействия
- **Deep dive**: детализация критичных компонентов
- **Trade-offs**: обоснование выбора между alternatives
- **Scalability**: планирование роста нагрузки
- **Bottlenecks**: идентификация и решение узких мест

**Типичный workflow интервью:**
```
1. Clarify requirements (5-10 min)
   - Functional: что система должна делать?
   - Non-functional: performance, scale, availability

2. High-level design (10-15 min)
   - Draw major components
   - Define data flow
   - Identify APIs

3. Deep dive (20-30 min)
   - Database design
   - Scalability решения
   - Caching strategy
   - Load balancing

4. Trade-offs и bottlenecks (10-15 min)
   - Discuss alternatives
   - Identify potential issues
   - Propose optimizations
```

**Принципы хорошего design:**
- **KISS** (Keep It Simple, Stupid): избегайте over-engineering
- **YAGNI** (You Aren't Gonna Need It): не добавляйте unnecessary features
- **DRY** (Don't Repeat Yourself): reuse components
- **Separation of Concerns**: разделение ответственности
- **Loose Coupling**: минимизация dependencies между компонентами
- **High Cohesion**: связанная functionality в одном месте

---

## Вопрос 2

**High-Level Design vs Low-Level Design**

High-Level Design (HLD) определяет overall архитектуру системы с major компонентами, их relationships, data flow и integration points без implementation details; Low-Level Design (LLD) детализирует каждый component с class diagrams, algorithms, data structures, APIs и конкретными технологиями; HLD даёт bird's-eye view для stakeholders, LLD — implementation roadmap для developers.

**High-Level Design (HLD):**

**Фокус:** Architecture, system components, data flow, external interfaces.

**Содержит:**
- System architecture diagram
- Component взаимодействия
- Database choice (SQL vs NoSQL)
- High-level APIs
- Third-party integrations
- Infrastructure (servers, load balancers)

**Пример HLD (E-commerce):**
```
Client (Web/Mobile)
      ↓
   CDN (Static content)
      ↓
Load Balancer
      ↓
┌─────────┬──────────┬──────────┐
│  Auth   │  Product │  Order   │
│ Service │  Service │ Service  │
└─────────┴──────────┴──────────┘
      ↓         ↓          ↓
┌──────────────────────────────┐
│     Message Queue (Kafka)     │
└──────────────────────────────┘
      ↓
┌─────────┬──────────┬──────────┐
│  User   │ Product  │  Order   │
│   DB    │    DB    │   DB     │
└─────────┴──────────┴──────────┘
      ↓
   Cache (Redis)
```

**Low-Level Design (LLD):**

**Фокус:** Classes, methods, algorithms, data structures, detailed APIs.

**Содержит:**
- Class diagrams
- Sequence diagrams
- Database schema (tables, columns, indexes)
- API contracts (request/response)
- Error handling
- Algorithm implementations

**Пример LLD (User Service):**
```
// Class design
public class UserService {
    private final UserRepository userRepository;
    private final PasswordEncoder passwordEncoder;
    private final JwtTokenProvider tokenProvider;
    
    public UserDTO registerUser(RegisterRequest request) {
        // Validate input
        validateRegistrationRequest(request);
        
        // Check if user exists
        if (userRepository.existsByEmail(request.getEmail())) {
            throw new UserAlreadyExistsException();
        }
        
        // Create user entity
        User user = User.builder()
            .email(request.getEmail())
            .password(passwordEncoder.encode(request.getPassword()))
            .name(request.getName())
            .createdAt(Instant.now())
            .build();
        
        // Save to database
        user = userRepository.save(user);
        
        // Return DTO
        return UserMapper.toDTO(user);
    }
}

// Database schema
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP,
    INDEX idx_email (email)
);
```

**Когда использовать:**
- **HLD**: Planning phase, stakeholder presentations, architecture reviews
- **LLD**: Implementation phase, code reviews, developer onboarding

---

## Вопрос 3

**Functional vs Non-Functional Requirements**

Functional Requirements описывают WHAT система должна делать (features, functionality, business logic): user registration, search products, place orders; Non-Functional Requirements определяют HOW система должна работать (quality attributes): performance, scalability, availability, security, usability; оба типа critical для successful system design, но non-functional часто определяют architecture choices.

**Functional Requirements:**

**Определение:** Specific behaviors или functions системы.

**Примеры (Twitter-like app):**
- User может создать account
- User может post tweets (max 280 characters)
- User может follow/unfollow других users
- User видит timeline с tweets от followed users
- User может like/retweet tweets
- Search tweets по keywords
- Trending topics отображение

**Как собирать:**
```
Вопросы interviewer:
- What features нужны в MVP?
- Who are the users?
- What are primary use cases?
- Any special business rules?
```

**Non-Functional Requirements:**

**Категории:**
- **Performance**: latency, throughput, response time
- **Scalability**: handle growth (users, data, requests)
- **Availability**: uptime (99.9%, 99.99%)
- **Reliability**: MTBF (Mean Time Between Failures)
- **Security**: authentication, authorization, encryption
- **Maintainability**: code quality, documentation
- **Usability**: user experience, accessibility

**Примеры (Twitter-like app):**
```
Performance:
- Timeline load < 200ms (p99)
- Tweet post latency < 100ms

Scalability:
- Support 100M daily active users
- Handle 10K tweets/second
- 1B tweets stored

Availability:
- 99.99% uptime (52 min downtime/year)
- No single point of failure

Consistency:
- Eventual consistency для timeline (допустимо)
- Strong consistency для payments
```

**Impact на design:**

**High availability requirement:**
```
→ Multi-region deployment
→ Database replication
→ Load balancing
→ Failover mechanisms
```

**High performance requirement:**
```
→ Caching (Redis/Memcached)
→ CDN для static content
→ Database indexing
→ Asynchronous processing
```

**High scalability requirement:**
```
→ Horizontal scaling
→ Database sharding
→ Microservices architecture
→ Message queues
```

**Prioritization:**

Не все требования equally important:
```
P0 (Must have): Core features, critical for MVP
P1 (Should have): Important but not blocking launch
P2 (Nice to have): Future enhancements
```

---

## Вопрос 4

**Scalability — что это?**

Scalability — это способность системы handle increased load (users, requests, data) через добавление resources без significant performance degradation; измеряется ability поддерживать throughput и latency при росте traffic; хорошо designed scalable система can grow from thousands к millions users с predictable costs и reasonable engineering efforts.

**Типы scalability:**

**1. Vertical Scaling (Scale Up):**
```
Добавление resources к existing machine:
- More CPU cores
- More RAM
- Faster storage (SSD → NVMe)
- Better network card

Pros:
✅ Simple implementation (no code changes)
✅ No distributed system complexity
✅ Consistent data (single machine)

Cons:
❌ Hardware limits (max CPU/RAM)
❌ Expensive (exponential cost)
❌ Single point of failure
❌ Downtime при upgrades

Example:
Database server: 4 CPU, 16GB RAM → 32 CPU, 256GB RAM
```

**2. Horizontal Scaling (Scale Out):**
```
Добавление more machines to cluster:
- Add application servers
- Add database replicas
- Add cache nodes

Pros:
✅ No theoretical limit
✅ Linear cost scaling
✅ Better fault tolerance
✅ No downtime (rolling updates)

Cons:
❌ Distributed system complexity
❌ Data consistency challenges
❌ Network latency
❌ Requires code changes

Example:
Web servers: 2 servers → 10 servers → 100 servers
```

**Scalability dimensions:**

**Request scalability:**
```
Handle increased number of requests/second

Solutions:
- Load balancing (distribute across servers)
- Caching (reduce backend load)
- Async processing (non-blocking)
- CDN (offload static content)
```

**Data scalability:**
```
Handle increased data volume

Solutions:
- Database sharding (horizontal partitioning)
- Database partitioning (vertical split)
- Data archiving (move old data)
- NoSQL databases (built for scale)
```

**User scalability:**
```
Handle increased concurrent users

Solutions:
- Stateless services (no session affinity)
- Session storage (Redis)
- Connection pooling
- WebSocket connections management
```

**Scalability metrics:**

```
Throughput: requests/second система может handle
Latency: response time под load
Resource utilization: CPU, memory, network usage
Cost per user: expenses при росте users
Error rate: failures при increased load
```

**Example calculation:**

```
Current: 1000 requests/sec, 100ms latency
Target: 10,000 requests/sec, <200ms latency

Vertical scaling:
1 server (32 cores) → 1 server (128 cores)
Cost: $500/mo → $5,000/mo (10x)

Horizontal scaling:
1 server → 10 servers (same specs)
Cost: $500/mo → $5,000/mo (10x)
But: better fault tolerance, no limits
```

**Scalability patterns:**

```
Stateless architecture: любой server handle любой request
Database read replicas: scale reads horizontally
Caching: reduce database load
Message queues: decouple components, async processing
CDN: offload static content globally
Microservices: scale services independently
```

---

## Вопрос 5

**Vertical Scaling vs Horizontal Scaling**

Vertical Scaling (scaling up) увеличивает capacity одного server через hardware upgrades (CPU, RAM, storage), simple to implement но limited by hardware maximum и creates SPOF; Horizontal Scaling (scaling out) добавляет more servers to distribute load, requires architectural changes (stateless design, load balancing) но provides unlimited scalability, better fault tolerance, и linear cost growth.

**Сравнение:**

| Aspect | Vertical Scaling | Horizontal Scaling |
|--------|-----------------|-------------------|
| **Implementation** | Upgrade hardware | Add more servers |
| **Complexity** | Low (no code changes) | High (distributed system) |
| **Cost** | Exponential growth | Linear growth |
| **Limit** | Hardware maximum | Theoretically unlimited |
| **Downtime** | Required for upgrade | Zero downtime possible |
| **Fault Tolerance** | SPOF risk | High (redundancy) |
| **Consistency** | Easy (single machine) | Complex (distributed data) |
| **Use Case** | Databases, legacy apps | Web servers, microservices |

**Vertical Scaling примеры:**

**Database scaling:**
```
PostgreSQL server upgrade:
Before: 8 cores, 32GB RAM, HDD
After: 64 cores, 512GB RAM, NVMe SSD

Results:
- 10x faster queries
- More concurrent connections
- Larger working set in memory
- But: still SPOF, expensive hardware
```

**Application server:**
```
Tomcat instance upgrade:
Before: 4 cores, 8GB RAM → handles 500 req/sec
After: 16 cores, 64GB RAM → handles 2000 req/sec

Pros: No load balancer needed, simpler ops
Cons: Cannot scale beyond 1 machine capacity
```

**Horizontal Scaling примеры:**

**Web tier scaling:**
```
           Load Balancer
                 ↓
    ┌────────────┼────────────┐
    ↓            ↓            ↓
Server 1      Server 2    Server 3
(4 cores)     (4 cores)   (4 cores)

Total capacity: 3x single server
Benefits:
- If Server 1 fails → traffic redirects to 2, 3
- Can add Server 4, 5, 6... as needed
- Rolling updates: no downtime
```

**Database scaling (read replicas):**
```
     Master DB (writes)
           ↓
     Replication
     ↓    ↓    ↓
Replica1 Replica2 Replica3
(reads) (reads)  (reads)

Read-heavy workload: scale reads infinitely
Write-heavy: still bottleneck at master
```

**Hybrid approach:**

Обычно используется combination:
```
1. Vertical scaling до reasonable limit
   Database: 32 cores, 256GB RAM

2. Horizontal scaling для дальнейшего роста
   Application: 10+ servers
   Cache: Redis cluster
   Database: sharding for writes
```

**Когда выбирать:**

**Vertical Scaling подходит когда:**
- Legacy applications (нельзя изменить architecture)
- Databases requiring strong consistency
- Quick fix для immediate capacity issue
- Small to medium scale (до hardware limits)

**Horizontal Scaling подходит когда:**
- Building new systems (can design for it)
- Need unlimited growth potential
- High availability critical
- Cost-effective long-term scaling

**Real-world example (Netflix):**
```
Origin: Single datacenter, vertical scaling
Problem: Hardware limits, SPOF, slow deploys

Solution: Horizontal scaling
- 1000s of microservices
- Multi-region deployment
- Auto-scaling based on load
- Chaos engineering (failure resilience)

Result: Handles 200M+ users globally
```

## Вопрос 6

**CAP Theorem**

CAP Theorem утверждает что distributed data store может гарантировать только 2 из 3 свойств одновременно: Consistency (все nodes видят одинаковые данные одновременно), Availability (каждый request получает response, success или failure), Partition Tolerance (система работает несмотря на network failures); в реальности partition tolerance обязательна (network failures неизбежны), поэтому выбор между CP (consistency при partition) или AP (availability при partition).

**Три свойства CAP:**

**Consistency (C):**
```
Все nodes видят одинаковые данные в один момент времени.
После successful write, все последующие reads видят это значение.

Example:
Write: balance = $100
Сразу после write, все replicas показывают $100
Ни одна replica не показывает старое значение
```

**Availability (A):**
```
Каждый request (read/write) получает response (не error).
Система operational даже если некоторые nodes failed.

Example:
3 database replicas, 1 failed
Requests продолжают обрабатываться оставшимися 2 replicas
```

**Partition Tolerance (P):**
```
Система продолжает работу при network partitions.
Network может разделить cluster на isolated groups.

Example:
Datacenter split: US-East не может connect к US-West
Обе части продолжают обрабатывать requests независимо
```

**CAP комбинации:**

**CP Systems (Consistency + Partition Tolerance):**
```
Выбор: Consistency важнее Availability

Behavior при partition:
- Minority partition становится unavailable (reject writes)
- Majority partition продолжает работу
- Гарантирует consistent data

Examples:
- MongoDB (с WriteConcern majority)
- HBase
- Redis (в CP mode)
- Zookeeper
- etcd

Use cases:
- Financial transactions
- Inventory management
- Any system где inconsistent data = corrupted data
```

**AP Systems (Availability + Partition Tolerance):**
```
Выбор: Availability важнее Consistency

Behavior при partition:
- Все partitions продолжают accept reads/writes
- Data может diverge между partitions
- Eventually consistent после partition heal

Examples:
- Cassandra
- DynamoDB
- Riak
- CouchDB

Use cases:
- Social media feeds
- Shopping carts
- Session storage
- Systems tolerating temporary inconsistency
```

**CA Systems (Consistency + Availability):**
```
Теоретически: нет partition tolerance
Практически: НЕ СУЩЕСТВУЕТ в distributed systems

Почему:
Network partitions НЕИЗБЕЖНЫ в distributed systems
CA возможна только в single-node systems

Example:
- Traditional RDBMS на single server (PostgreSQL, MySQL)
- Но это не distributed system!
```

**Real-world trade-offs:**

**Banking system (CP):**
```
// MongoDB с strong consistency
@Transactional
public void transfer(String from, String to, BigDecimal amount) {
    Account fromAccount = accountRepo.findById(from);
    Account toAccount = accountRepo.findById(to);
    
    if (fromAccount.getBalance().compareTo(amount) < 0) {
        throw new InsufficientFundsException();
    }
    
    fromAccount.setBalance(fromAccount.getBalance().subtract(amount));
    toAccount.setBalance(toAccount.getBalance().add(amount));
    
    // Both updates или оба rollback (consistency)
    // При partition: minority nodes reject writes (sacrifice availability)
    accountRepo.save(fromAccount);
    accountRepo.save(toAccount);
}
```

**Social media feed (AP):**
```
// Cassandra с eventual consistency
public void postTweet(String userId, String content) {
    Tweet tweet = new Tweet(userId, content, Instant.now());
    
    // Write accepted даже при partition
    // Может быть временно inconsistent между datacenters
    // Eventually все replicas converge
    tweetRepository.save(tweet);
    
    // Users могут видеть slightly stale data
    // Acceptable для social media
}
```

**PACELC Extension:**

```
PACELC = расширение CAP для нормальной работы (без partitions)

If Partition:
  Choose between Availability and Consistency
Else (normal operation):
  Choose between Latency and Consistency

Examples:
PA/EL: Cassandra (AP system, low latency в normal mode)
PC/EC: HBase (CP system, high consistency в normal mode)
PA/EC: DynamoDB (AP system, но можно выбрать consistency)
```

**Как выбрать:**

```
Вопросы для decision:
1. Can система tolerate stale data?
   Yes → AP (Cassandra, DynamoDB)
   No → CP (MongoDB, HBase)

2. Is availability critical?
   Yes → AP (always respond)
   No → CP (better consistent than available)

3. Can you handle conflicts?
   Yes → AP (conflict resolution logic)
   No → CP (avoid conflicts entirely)
```

---

## Вопрос 7

**ACID vs BASE**

ACID (Atomicity, Consistency, Isolation, Durability) — это set of properties гарантирующий reliable transaction processing в traditional databases с strong consistency; BASE (Basically Available, Soft state, Eventually consistent) — alternative model для distributed systems жертвующий immediate consistency ради availability и partition tolerance; ACID подходит для financial systems requiring correctness, BASE — для high-scale systems tolerating temporary inconsistencies.

**ACID Properties:**

**Atomicity:**
```
Transaction выполняется полностью или не выполняется вообще.
All-or-nothing guarantee.

Example:
BEGIN TRANSACTION;
  UPDATE accounts SET balance = balance - 100 WHERE id = 1;
  UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

Если любой UPDATE fails → оба rollback
Не может быть partial state (деньги пропали/удвоились)
```

**Consistency:**
```
Transaction переводит database из одного valid state в другой.
Business rules и constraints сохраняются.

Example:
Constraint: balance >= 0

BEGIN TRANSACTION;
  UPDATE accounts SET balance = balance - 200 WHERE id = 1;
  -- Если balance стал отрицательным → ROLLBACK
COMMIT;

Database остаётся в consistent state
```

**Isolation:**
```
Concurrent transactions не влияют друг на друга.
Результат concurrent execution = sequential execution.

Example:
Transaction 1: Transfer $100 from A to B
Transaction 2: Transfer $50 from A to C

Isolation levels:
- Read Uncommitted (dirty reads)
- Read Committed (no dirty reads)
- Repeatable Read (consistent reads)
- Serializable (полная изоляция)
```

**Durability:**
```
После COMMIT изменения permanent (даже при crash).
Данные сохранены на disk, не lost при power failure.

Implementation:
- Write-Ahead Log (WAL)
- Fsync to disk
- Replication
```

**BASE Properties:**

**Basically Available:**
```
System гарантирует availability (может быть degraded).
Partial failures допустимы, система продолжает работу.

Example:
3-node Cassandra cluster:
- 1 node down
- System продолжает работу с 2 nodes
- Возвращает best available data
- Может быть slightly stale
```

**Soft State:**
```
System state может изменяться со временем даже без input.
Due to eventual consistency model.

Example:
После write в node1, replicas eventually updated:
t=0: node1=100, node2=90, node3=90 (inconsistent)
t=1: node1=100, node2=100, node3=90 (propagating)
t=2: node1=100, node2=100, node3=100 (consistent)
```

**Eventually Consistent:**
```
Если нет новых updates, все replicas eventually converge.
Consistency achieved over time, не immediately.

Example:
User posts tweet:
- Written to local datacenter
- Asynchronously replicated globally
- Users в других regions видят with delay (seconds)
- Eventually все видят tweet
```

**Сравнение:**

| Aspect | ACID | BASE |
|--------|------|------|
| **Consistency** | Strong, immediate | Eventual |
| **Availability** | May sacrifice for consistency | High, always available |
| **Latency** | Higher (sync operations) | Lower (async replication) |
| **Scalability** | Vertical scaling | Horizontal scaling |
| **Use Case** | Banking, inventory | Social media, caching |
| **Examples** | PostgreSQL, MySQL | Cassandra, DynamoDB |

**ACID example (Banking):**
```
@Transactional(isolation = Isolation.SERIALIZABLE)
public void transferMoney(Long fromId, Long toId, BigDecimal amount) {
    Account from = accountRepo.findById(fromId)
        .orElseThrow(() -> new AccountNotFoundException());
    Account to = accountRepo.findById(toId)
        .orElseThrow(() -> new AccountNotFoundException());
    
    // Atomicity: both updates или nothing
    from.withdraw(amount); // Consistency: checks balance >= amount
    to.deposit(amount);
    
    accountRepo.save(from);
    accountRepo.save(to);
    
    // Isolation: другие transactions не видят intermediate state
    // Durability: после commit данные persistent
}
```

**BASE example (Social Media):**
```
public void likeTweet(String userId, String tweetId) {
    // Write to local datacenter (fast response)
    LikeEvent event = new LikeEvent(userId, tweetId, Instant.now());
    likeEventStore.save(event); // Basically Available
    
    // Increment counter асинхронно
    CompletableFuture.runAsync(() -> {
        tweetStatsService.incrementLikes(tweetId);
    });
    
    // Replicate to other datacenters асинхронно
    replicationService.replicateAsync(event); // Eventually Consistent
    
    // User видит like immediately locally
    // Other users видят like with slight delay (acceptable)
    // System remains available даже если remote datacenters down
}
```

**Hybrid approaches:**

Многие modern systems используют mix:
```
// Cassandra с tunable consistency
public void writeData(String key, String value) {
    // Critical data: QUORUM consistency (более ACID-like)
    session.execute(
        boundStatement,
        ConsistencyLevel.QUORUM
    );
    
    // Non-critical data: ONE consistency (BASE-like)
    session.execute(
        boundStatement,
        ConsistencyLevel.ONE
    );
}
```

**Когда выбирать:**

**ACID для:**
- Financial transactions
- Inventory management
- Booking systems
- Anywhere где inconsistency = data corruption

**BASE для:**
- Social media feeds
- Analytics data
- Caching
- Session storage
- High-scale read-heavy systems

---

## Вопрос 8

**Latency vs Throughput**

Latency — это time taken для single operation completion (response time), измеряется в milliseconds/seconds и critical для user experience; Throughput — это number of operations processed per time unit (requests/second), измеряет system capacity; обычно trade-off между ними: optimizing для low latency может reduce throughput и наоборот; good system design balances оба metrics based на requirements.

**Latency:**

**Definition:**
```
Time from request sent до response received.
Measures: How fast is single operation?

Components:
- Network latency (client ↔ server)
- Queue waiting time
- Processing time
- Database query time
- External API calls
```

**Latency metrics:**
```
Average (mean): может скрывать outliers
Median (p50): 50% requests быстрее этого
p95: 95% requests быстрее (5% slower)
p99: 99% requests быстрее (1% slower)
p99.9: tail latency

Example:
p50 = 10ms (median user experience)
p95 = 50ms (95% users get < 50ms)
p99 = 200ms (worst 1% get 200ms+)
p99.9 = 1s (extremely slow outliers)
```

**Throughput:**

**Definition:**
```
Number of operations processed per unit time.
Measures: How many operations can system handle?

Units:
- Requests/second (RPS)
- Queries/second (QPS)
- Transactions/second (TPS)
- Messages/second

Example:
Web server: 10,000 requests/second
Database: 50,000 queries/second
Message queue: 1M messages/second
```

**Throughput factors:**
```
= (Available Resources) / (Resources per Request)

Resources:
- CPU cores
- Memory
- Network bandwidth
- Disk I/O

Resource per request:
- Processing time
- Memory allocation
- DB connections
```

**Trade-offs:**

**Scenario 1: Optimizing for latency:**
```
// Prioritize speed of individual requests
@RestController
public class UserController {
    
    @GetMapping("/user/{id}")
    public User getUser(@PathVariable Long id) {
        // Direct database call (fast for single request)
        return userService.findById(id); // 5ms latency
        
        // No batching, no caching (для freshest data)
        // Low latency (5ms) но lower throughput
        // Each request individual DB hit
    }
}

Результат:
Latency: 5ms (good!)
Throughput: 1000 req/sec (limited by DB connections)
```

**Scenario 2: Optimizing for throughput:**
```
// Prioritize number of requests handled
@RestController
public class UserController {
    
    @GetMapping("/users/batch")
    public List<User> getUsers(@RequestParam List<Long> ids) {
        // Batch processing (efficient для throughput)
        return userService.findByIds(ids); // 50ms для batch of 100
        
        // Single DB query для multiple users
        // Latency per user: 50ms / 100 = 0.5ms
        // But client waits 50ms для всей batch
    }
}

Результат:
Latency: 50ms для batch (higher per request)
Throughput: 10,000 req/sec (10x improvement через batching)
```

**Real-world examples:**

**Low latency system (Trading):**
```
Requirement: Execute trade < 1ms
Solution:
- Co-located servers (same datacenter as exchange)
- In-memory processing (no disk I/O)
- Direct network connections (no proxy/LB)
- Optimized code (assembly, FPGA)

Result:
Latency: 0.1ms (excellent!)
Throughput: 1,000 trades/sec (lower, но acceptable)

Reason: Each trade requires dedicated resources
Cannot batch trades (each unique)
```

**High throughput system (Analytics):**
```
Requirement: Process 1M events/second
Solution:
- Batch processing (100 events per batch)
- Async processing (queue-based)
- Horizontal scaling (100 workers)
- Columnar storage (efficient scanning)

Result:
Latency: 10 seconds (batch + processing)
Throughput: 1M events/sec (excellent!)

Reason: Batching improves efficiency
Latency acceptable для analytics (не real-time)
```

**Improving both:**

Возможно улучшить оба metrics одновременно:
```
// Caching reduces latency AND increases throughput
@Service
public class UserService {
    
    @Cacheable(value = "users", key = "#id")
    public User findById(Long id) {
        return userRepository.findById(id).orElseThrow();
    }
}

Without cache:
Latency: 50ms (DB query)
Throughput: 1,000 req/sec (DB limit)

With cache (90% hit rate):
Latency: 5ms average (1ms cache + 10% × 50ms DB)
Throughput: 10,000 req/sec (cache handles most)

Both metrics improved!
```

**Connection pooling:**
```
// Reuse connections reduces latency AND increases throughput
@Configuration
public class DataSourceConfig {
    
    @Bean
    public DataSource dataSource() {
        HikariConfig config = new HikariConfig();
        config.setMaximumPoolSize(50); // Reuse connections
        config.setMinimumIdle(10);
        config.setConnectionTimeout(5000);
        
        return new HikariDataSource(config);
    }
}

Without pooling:
Latency: 100ms (50ms connection + 50ms query)
Throughput: 100 req/sec (connection bottleneck)

With pooling:
Latency: 50ms (reuse connection, только query)
Throughput: 1,000 req/sec (no connection overhead)
```

**Measuring:**

```
// Latency measurement
long start = System.nanoTime();
processRequest(request);
long latency = System.nanoTime() - start;
// Report: p50, p95, p99

// Throughput measurement
AtomicLong requestCount = new AtomicLong(0);
ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);
scheduler.scheduleAtFixedRate(() -> {
    long count = requestCount.getAndSet(0);
    double throughput = count / 10.0; // per second
    System.out.println("Throughput: " + throughput + " req/sec");
}, 0, 10, TimeUnit.SECONDS);
```

---

## Вопрос 9

**Consistency Models**

Consistency Models определяют guarantees о порядке и видимости operations в distributed system: Strong Consistency гарантирует immediate visibility всех writes across all nodes (ACID-like); Eventual Consistency допускает temporary inconsistencies, converging over time; Causal Consistency сохраняет cause-effect relationships; выбор model влияет на performance, availability и complexity системы.

**Strong Consistency (Linearizability):**

**Определение:**
```
After write completes, все последующие reads видят новое значение.
Система ведёт себя как single copy of data.
Эквивалентно single-server behavior.
```

**Guarantees:**
```
Write(x = 1) at t1 completes
Read(x) at t2 > t1 returns 1 (always)

Real-time ordering preserved
Latest value always returned
```

**Implementation:**
```
// Distributed lock для strong consistency
public class StronglyConsistentCounter {
    private final DistributedLock lock;
    private final Database database;
    
    public void increment(String key) {
        lock.acquire(key); // Блокирует все nodes
        try {
            int value = database.get(key);
            database.set(key, value + 1);
            // All replicas updated synchronously
            database.replicateSync();
        } finally {
            lock.release(key);
        }
    }
    
    public int get(String key) {
        // Reads всегда from latest replica
        return database.readFromMaster(key);
    }
}
```

**Use cases:**
- Banking transactions
- Inventory systems
- Booking/reservation systems
- Distributed locks

**Pros/Cons:**
```
✅ Simple reasoning (как single server)
✅ No conflicts to resolve
✅ Correct by definition

❌ High latency (sync replication)
❌ Lower availability (при partitions)
❌ Limited scalability
```

**Eventual Consistency:**

**Определение:**
```
Если нет новых writes, все replicas eventually converge.
Temporary inconsistencies допустимы.
No guarantees на timing convergence.
```

**Guarantees:**
```
Write(x = 1) at t1
Read(x) at t2 may return 0 (stale)
Read(x) at t3 may return 1 (updated)
Eventually all reads return 1
```

**Implementation:**
```
// Async replication для eventual consistency
public class EventuallyConsistentCache {
    private final LocalCache localCache;
    private final ReplicationService replicationService;
    
    public void set(String key, String value) {
        // Write locally (fast)
        localCache.set(key, value);
        
        // Replicate асинхронно
        replicationService.replicateAsync(key, value);
        
        // Client видит write immediately locally
        // Other nodes видят with delay
    }
    
    public String get(String key) {
        // May return stale value
        return localCache.get(key);
    }
}
```

**Use cases:**
- Social media feeds
- DNS
- Caching
- Session storage
- Analytics data

**Pros/Cons:**
```
✅ Low latency (local writes)
✅ High availability
✅ High scalability

❌ Complex reasoning (conflicts possible)
❌ Stale reads
❌ Conflict resolution needed
```

**Causal Consistency:**

**Определение:**
```
Preserves cause-effect relationships.
Если operation A caused operation B, все nodes видят A before B.
Concurrent operations могут быть видны в разном порядке.
```

**Example:**
```
User posts tweet (A)
User deletes tweet (B) - caused by A

Causal consistency гарантирует:
✅ Delete never visible before post
✅ All users видят post then delete (в этом порядке)

Но concurrent posts от разных users:
❌ May appear в разном порядке на разных nodes
```

**Implementation:**
```
// Vector clocks для causal consistency
public class CausallyConsistentStore {
    private final Map<String, Versioned<String>> data;
    private final VectorClock vectorClock;
    
    public void write(String key, String value) {
        vectorClock.increment(); // Track causality
        Versioned<String> versioned = new Versioned<>(
            value, 
            vectorClock.copy()
        );
        data.put(key, versioned);
    }
    
    public String read(String key) {
        Versioned<String> versioned = data.get(key);
        // Return value if causal dependencies met
        return versioned.getValue();
    }
}
```

**Read Your Own Writes:**

```
User writes data
User immediately reads same data
Guarantee: видит свой own write

Implementation:
- Session affinity (route to same server)
- Client caching
- Read from master after write
```

**Monotonic Reads:**

```
User reads value at t1
User reads same value at t2 > t1
Guarantee: value at t2 >= value at t1 (не стареет)

Implementation:
- Read from same replica
- Track version numbers
```

**Consistency vs Performance:**

```
Strong Consistency:
├─ Latency: High (sync replication)
├─ Throughput: Low (coordination overhead)
├─ Availability: Lower (quorum required)
└─ Use case: Financial systems

Eventual Consistency:
├─ Latency: Low (async replication)
├─ Throughput: High (no coordination)
├─ Availability: High (always writable)
└─ Use case: Social media

Causal Consistency:
├─ Latency: Medium (track dependencies)
├─ Throughput: Medium (some overhead)
├─ Availability: Medium
└─ Use case: Collaborative editing
```

**Tunable consistency (Cassandra):**
```
// Application выбирает consistency level per request
public void writeData(String key, String value, boolean critical) {
    ConsistencyLevel level = critical 
        ? ConsistencyLevel.QUORUM  // Strong-ish
        : ConsistencyLevel.ONE;    // Weak

    session.execute(
        insertStatement.bind(key, value),
        level
    );
}
```

---

## Вопрос 10

**Availability и High Availability**

Availability — это percentage времени, когда система operational и accessible, измеряется как uptime ratio (99.9% = 8.76 hours downtime/year); High Availability (HA) — design approach обеспечивающий availability through redundancy, failover, load balancing и elimination of single points of failure; достигается через multi-region deployment, database replication, health checks, и automated recovery mechanisms.

**Availability metrics:**

**Uptime percentage:**
```
Availability = (Total Time - Downtime) / Total Time × 100%

99% ("two nines"):
Downtime: 3.65 days/year, 7.2 hours/month
Acceptable: Basic web services

99.9% ("three nines"):
Downtime: 8.76 hours/year, 43 minutes/month
Acceptable: Business applications

99.99% ("four nines"):
Downtime: 52 minutes/year, 4.3 minutes/month
Required: E-commerce, banking

99.999% ("five nines"):
Downtime: 5.26 minutes/year, 25 seconds/month
Required: Critical infrastructure, telecom
```

**Calculating availability:**
```
Series (components в chain):
Total = A1 × A2 × A3

Example: Load Balancer (99.9%) → App Server (99.9%) → DB (99.9%)
Total = 0.999 × 0.999 × 0.999 = 0.997 = 99.7%
Worse than individual components!

Parallel (redundant components):
Total = 1 - (1 - A1) × (1 - A2)

Example: 2 App Servers (99.9% each) in parallel
Total = 1 - (1 - 0.999) × (1 - 0.999)
      = 1 - 0.001 × 0.001
      = 1 - 0.000001 = 0.999999 = 99.9999%
Much better with redundancy!
```

**High Availability Architecture:**

**Redundancy:**
```
Multiple instances of each component
No single point of failure (SPOF)

Example:
┌─────────────┐
│ Load Balancer│ (HAProxy with keepalived)
└──────┬──────┘
       │
   ┌───┴───┬────────┐
   ↓       ↓        ↓
Server1  Server2  Server3  (Application tier - 3 replicas)
   │       │        │
   └───┬───┴────────┘
       ↓
  ┌────────┐
  │ Master │ ←→ Standby  (Database replication)
  └────────┘

Any single component can fail без impact на availability
```

**Failover mechanisms:**
```
// Automatic failover для database
@Configuration
public class DatabaseFailoverConfig {
    
    @Bean
    public DataSource dataSource() {
        // Primary database
        DataSource primary = createDataSource("primary-db.example.com");
        
        // Standby replicas
        DataSource standby1 = createDataSource("standby1-db.example.com");
        DataSource standby2 = createDataSource("standby2-db.example.com");
        
        // Failover routing
        return new FailoverDataSource(primary, 
            Arrays.asList(standby1, standby2));
    }
}

// FailoverDataSource автоматически switches при failure
// Transparent для application code
```

**Health checks:**
```
// Kubernetes liveness/readiness probes
@RestController
public class HealthController {
    
    @GetMapping("/health/live")
    public ResponseEntity<String> liveness() {
        // Basic check: is application running?
        return ResponseEntity.ok("UP");
    }
    
    @GetMapping("/health/ready")
    public ResponseEntity<String> readiness() {
        // Detailed checks: DB, cache, dependencies
        if (!isDatabaseConnected()) {
            return ResponseEntity.status(503).body("DB DOWN");
        }
        if (!isCacheAvailable()) {
            return ResponseEntity.status(503).body("CACHE DOWN");
        }
        return ResponseEntity.ok("READY");
    }
}

// Load balancer removes unhealthy instances
// Kubernetes restarts failed pods
```

**Multi-region deployment:**
```
Primary Region: US-East
┌──────────────────────┐
│  Load Balancer       │
│  App Servers (3x)    │
│  Database (Master)   │
└──────────────────────┘
         ↕ (Replication)
Secondary Region: US-West
┌──────────────────────┐
│  Load Balancer       │
│  App Servers (3x)    │
│  Database (Replica)  │
└──────────────────────┘

Benefits:
- Disaster recovery (entire region failure)
- Lower latency (geo-distributed)
- Load distribution

Failover: DNS/Global Load Balancer switches regions
```

**Database replication for HA:**
```
Master-Slave:
Master ──write──> Slave1
  │               Slave2
  │               Slave3
  └── If master fails: promote slave to master

Master-Master:
Master1 ↔ Master2
Both accept writes (active-active)
Conflict resolution required

Quorum-based (Cassandra):
Write to majority of replicas
Any replica can serve reads
Survives minority of failures
```

**Load balancer HA:**
```
Primary LB (active)  ←→  Secondary LB (standby)
       ↓                      ↓
   Virtual IP (keepalived)
       
Primary handles traffic
If primary fails: secondary takes over Virtual IP
Failover time: < 1 second
```

**Achieving 99.99% availability:**

```
Strategies:
1. Redundancy: 3+ instances of each component
2. Automated failover: < 30 seconds
3. Health monitoring: continuous checks
4. Graceful degradation: partial functionality if dependency down
5. Circuit breakers: prevent cascade failures
6. Multi-AZ deployment: survive datacenter failure
7. Zero-downtime deploys: rolling updates
8. Automated recovery: self-healing
9. Capacity planning: handle traffic spikes
10. Regular DR drills: test failover procedures

Cost:
Higher availability = higher cost
99.9% → 99.99%: 2-3x infrastructure cost
99.99% → 99.999%: 5-10x cost
```

**Graceful degradation example:**
```
@Service
public class ProductService {
    
    public ProductDetails getProduct(Long id) {
        try {
            // Try primary source (full details)
            return externalApiClient.getProductDetails(id);
        } catch (ExternalApiException e) {
            // Fallback to cache (stale но available)
            ProductDetails cached = cache.get(id);
            if (cached != null) {
                return cached.withWarning("Data may be stale");
            }
            
            // Fallback to basic details from local DB
            return database.getBasicProductInfo(id);
        }
    }
}

// System остаётся available даже при failures
// Degraded functionality лучше чем complete outage
```

## Вопрос 11

**Single Point of Failure (SPOF)**

SPOF — это компонент системы, failure которого приводит к полной недоступности системы; примеры: single database server, единственный load balancer, central message queue без replication; устраняется через redundancy (multiple instances), replication (data copies), failover mechanisms, и distributed architecture eliminating dependency на single component.

**Примеры SPOF:**
```
❌ Single Database Server:
   App → Single DB
   DB fails = entire system down

✅ With Replication:
   App → Load Balancer → DB Master
                      → DB Replica1
                      → DB Replica2

❌ Single Load Balancer:
   Clients → Single LB → Servers
   LB fails = no routing

✅ HA Load Balancers:
   Clients → Virtual IP (keepalived)
          → Primary LB (active)
          → Secondary LB (standby)
```

---

## Вопрос 12

**Fault Tolerance и Resilience**

Fault Tolerance — способность системы continue operating при component failures без service interruption; Resilience — способность recover from failures quickly и adapt to changing conditions; достигается через redundancy, graceful degradation, circuit breakers, retry logic, timeouts, bulkheads (isolation), и chaos engineering testing.

**Patterns:**
```
// Circuit Breaker
@Service
public class ResilientService {
    
    @CircuitBreaker(name = "externalApi", fallbackMethod = "fallback")
    public String callExternalApi() {
        return externalApiClient.getData();
    }
    
    private String fallback(Exception e) {
        return cacheService.getCachedData();
    }
}

// Retry with exponential backoff
@Retryable(
    maxAttempts = 3,
    backoff = @Backoff(delay = 1000, multiplier = 2)
)
public Data fetchData() {
    return externalService.getData();
}

// Bulkhead (thread pool isolation)
@Bulkhead(name = "databaseOperations", type = Bulkhead.Type.THREADPOOL)
public Result queryDatabase() {
    return database.query();
}
```

---

## Вопрос 13

**Load Balancer — зачем нужен?**

Load Balancer — это компонент распределяющий incoming traffic между multiple servers для horizontal scaling, high availability, и optimal resource utilization; предотвращает overload одного server, provides health checking (removes failed servers), enables zero-downtime deployments, и может work на L4 (TCP/UDP) или L7 (HTTP/HTTPS) level.

**Типы:**
```
L4 Load Balancer (Transport Layer):
- Routes based на IP/Port
- Fast (no packet inspection)
- Protocol agnostic
- Examples: HAProxy (TCP mode), AWS NLB

L7 Load Balancer (Application Layer):
- Routes based на HTTP headers, URL, cookies
- Content-based routing
- SSL termination
- Examples: Nginx, HAProxy (HTTP mode), AWS ALB
```

**Algorithms:**
```
Round Robin: requests sequentially (server1→server2→server3→server1)
Least Connections: route to server с fewest active connections
IP Hash: consistent routing (same client → same server)
Weighted Round Robin: distribute based на server capacity
Least Response Time: route to fastest responding server
```

---

## Вопрос 14

**Reverse Proxy vs Forward Proxy**

Reverse Proxy sits перед backend servers и forwards client requests to appropriate server, providing load balancing, SSL termination, caching, security (hiding backend topology); Forward Proxy sits перед clients и forwards their requests to internet, providing anonymity, content filtering, caching for clients; Reverse Proxy server-side (protects backends), Forward Proxy client-side (protects clients).

**Reverse Proxy:**
```
Client → Reverse Proxy → Backend Servers

Features:
- Load balancing
- SSL termination
- Caching
- Compression
- Security (WAF)
- Hide backend IPs

Examples: Nginx, HAProxy, AWS ALB
Use case: Production web applications
```

**Forward Proxy:**
```
Client → Forward Proxy → Internet

Features:
- Anonymity (hide client IP)
- Content filtering
- Caching (reduce bandwidth)
- Access control

Examples: Squid, Corporate proxies
Use case: Corporate networks, privacy
```

---

## Вопрос 15

**API Gateway**

API Gateway — это single entry point для всех client requests в microservices architecture, предоставляющий routing, authentication, rate limiting, request/response transformation, protocol translation, aggregation (combining multiple service calls), monitoring, и caching; abstracts internal architecture от clients, reducing coupling и enabling independent service evolution.

**Функции:**
```
@Component
public class ApiGateway {
    
    // 1. Authentication/Authorization
    @PreAuthorize("hasRole('USER')")
    public Response handleRequest(Request request) {
        
        // 2. Rate Limiting
        if (rateLimiter.isExceeded(request.getUserId())) {
            return Response.status(429).build();
        }
        
        // 3. Request Routing
        String service = routeToService(request.getPath());
        
        // 4. Load Balancing
        ServiceInstance instance = loadBalancer.choose(service);
        
        // 5. Circuit Breaking
        return circuitBreaker.execute(() -> 
            instance.call(request)
        );
    }
    
    // 6. Response Aggregation
    public UserProfile aggregateUserData(Long userId) {
        CompletableFuture<User> userFuture = 
            userService.getUser(userId);
        CompletableFuture<Orders> ordersFuture = 
            orderService.getOrders(userId);
        CompletableFuture<Preferences> prefFuture = 
            preferenceService.getPreferences(userId);
            
        return CompletableFuture.allOf(userFuture, ordersFuture, prefFuture)
            .thenApply(v -> new UserProfile(
                userFuture.join(),
                ordersFuture.join(),
                prefFuture.join()
            ));
    }
}
```

**Popular gateways:** Kong, AWS API Gateway, Spring Cloud Gateway, Nginx, Envoy.

---

## Вопрос 16

**Monolithic Architecture**

Monolithic Architecture — это traditional approach где entire application built как single deployable unit с shared codebase, database, и memory space; все components (UI, business logic, data access) deployed together; simple для small applications но challenges с scaling (must scale entire app), deployment (any change requires full redeploy), technology lock-in, и team coordination для large codebases.

**Характеристики:**
```
Single codebase
Single deployment unit
Shared database
Tightly coupled components
Single technology stack

Example Structure:
my-app.jar
├── Controllers (REST APIs)
├── Services (Business Logic)
├── Repositories (Data Access)
├── Models (Domain Objects)
└── Configuration
```

**Pros:**
```
✅ Simple development (single codebase)
✅ Easy debugging (single process)
✅ Simple deployment (one artifact)
✅ Easy testing (no distributed system complexity)
✅ Better performance (no network calls)
```

**Cons:**
```
❌ Scaling challenges (must scale entire app)
❌ Deployment risk (small change = full redeploy)
❌ Technology lock-in (one stack для всего)
❌ Large codebase (hard to navigate)
❌ Long build times
❌ Team coordination (merge conflicts)
```

---

## Вопрос 17

**Microservices Architecture**

Microservices Architecture разбивает application на independent, loosely coupled services каждый responsible за specific business capability, deployed independently, с own database, communicating через APIs (REST, gRPC, messaging); enables independent scaling, technology diversity, faster deployments, team autonomy, но добавляет distributed system complexity, network latency, data consistency challenges.

**Характеристики:**
```
Independent services
Separate databases (database per service)
Decentralized governance
API-based communication
Independent deployment
Technology diversity

Example:
User Service (Java/Spring)
Order Service (Go)
Payment Service (Node.js)
Notification Service (Python)
```

**Communication:**
```
Synchronous: REST APIs, gRPC
Asynchronous: Message queues (Kafka, RabbitMQ)
Service discovery: Consul, Eureka
API Gateway: Kong, AWS API Gateway
```

**Pros:**
```
✅ Independent scaling
✅ Independent deployment
✅ Technology diversity
✅ Team autonomy
✅ Fault isolation
✅ Easier to understand (smaller codebases)
```

**Cons:**
```
❌ Distributed system complexity
❌ Network latency
❌ Data consistency challenges
❌ More infrastructure overhead
❌ Debugging difficulty
❌ Transaction management
```

---

## Вопрос 18-30

**(Краткие ответы для экономии места)**

**18. SOA:** Service-Oriented Architecture с enterprise service bus (ESB), heavy protocols (SOAP/XML), coarse-grained services, shared database, centralized governance.

**19. Event-Driven Architecture:** Асинхронная communication через events, producers publish events, consumers subscribe, decoupling, eventual consistency, examples: Kafka, RabbitMQ.

**20. CQRS Pattern:** Command Query Responsibility Segregation, separate models для reads (queries) и writes (commands), optimized independently, eventual consistency между models.

**21. Saga Pattern:** Distributed transaction management через sequence of local transactions, each with compensating transaction для rollback, координация через orchestration (central coordinator) или choreography (event-based).

**22. Circuit Breaker:** Prevents cascade failures, states: Closed (normal), Open (failing, reject requests), Half-Open (test recovery), автоматический fallback to cached data.

**23. Strangler Fig Pattern:** Gradually migrate monolith to microservices, routing layer directs traffic, new features in microservices, old features gradually migrated, zero-downtime migration.

**24. Sidecar Pattern:** Auxiliary container/process alongside main application, handles cross-cutting concerns (logging, monitoring, service mesh proxy), examples: Envoy, Istio.

**25. Backend for Frontend (BFF):** Separate backend для каждого frontend type (web, mobile, desktop), optimized для specific client needs, aggregates multiple service calls.

**26. API Gateway Pattern:** Single entry point для all clients, routing, authentication, rate limiting, request transformation, response aggregation, protocol translation.

**27. Service Mesh:** Infrastructure layer managing service-to-service communication, sidecar proxies (Envoy), traffic management, security (mTLS), observability, examples: Istio, Linkerd.

**28. Database per Service:** Each microservice owns private database, full autonomy, independent scaling, data consistency через Saga or event sourcing.

**29. Shared Database:** Multiple services access same database, simple но tight coupling, scaling challenges, schema changes affect all services, anti-pattern для microservices.

**30. Two-Phase Commit vs Saga:** 2PC - blocking distributed transaction protocol с coordinator, всё или ничего; Saga - asynchronous compensating transactions, eventual consistency, better availability.

---

## Вопрос 31

**SQL vs NoSQL**

SQL databases (relational) используют structured schema, ACID transactions, table joins, vertical scaling, best для complex queries и strong consistency; NoSQL databases (document, key-value, column-family, graph) schema-flexible, eventual consistency, horizontal scaling, optimized для specific data models, best для high-scale read/write operations, semi-structured data.

**Сравнение:**

| Feature | SQL | NoSQL |
|---------|-----|-------|
| **Schema** | Fixed, predefined | Flexible, dynamic |
| **Scaling** | Vertical (limited) | Horizontal (unlimited) |
| **Consistency** | Strong (ACID) | Eventual (BASE) |
| **Joins** | Built-in | Application-level |
| **Transactions** | Multi-row ACID | Limited/none |
| **Query Language** | SQL (standardized) | API-specific |
| **Use Cases** | Banking, ERP | Social media, IoT |
| **Examples** | PostgreSQL, MySQL | MongoDB, Cassandra |

**SQL примеры:**
```
-- Complex joins, transactions
BEGIN TRANSACTION;
INSERT INTO orders (user_id, total) VALUES (1, 100);
INSERT INTO order_items (order_id, product_id, qty) VALUES (LAST_INSERT_ID(), 5, 2);
UPDATE products SET stock = stock - 2 WHERE id = 5;
COMMIT;

-- ACID guarantees
```

**NoSQL примеры:**
```
// MongoDB - Document store
db.users.insertOne({
  _id: ObjectId(),
  name: "John",
  orders: [ // Embedded data (denormalized)
    { orderId: 1, total: 100, items: [...] }
  ],
  preferences: { theme: "dark", language: "en" }
});

// Flexible schema, no joins needed
```

**Когда SQL:**
- Complex relationships и joins
- ACID transactions required
- Structured, stable schema
- Business intelligence, reporting
- Financial systems

**Когда NoSQL:**
- High-scale writes/reads
- Flexible, evolving schema
- Horizontal scaling needed
- Key-value lookups
- Time-series data, logs

---

## Вопрос 32

**Database Indexing**

Database Index — это data structure (обычно B-Tree) ускоряющий search operations через создание sorted reference to table rows; reads faster (O(log n) вместо O(n) full table scan) но writes slower (must update index), indexes consume storage, over-indexing degrades performance, выбирать columns для indexing based на query patterns.

**Типы:**
```
-- Primary Key Index (automatic, unique, clustered)
CREATE TABLE users (
  id SERIAL PRIMARY KEY,  -- Auto-indexed
  email VARCHAR(255),
  name VARCHAR(255)
);

-- Unique Index (enforce uniqueness)
CREATE UNIQUE INDEX idx_email ON users(email);

-- Regular Index (speed up queries)
CREATE INDEX idx_name ON users(name);

-- Composite Index (multiple columns)
CREATE INDEX idx_name_email ON users(name, email);

-- Partial Index (filtered)
CREATE INDEX idx_active_users ON users(email) WHERE active = true;

-- Full-Text Index (text search)
CREATE FULLTEXT INDEX idx_description ON products(description);
```

**Использование:**
```
-- Without index: Full table scan O(n)
SELECT * FROM users WHERE email = 'john@example.com';
-- Scans all rows

-- With index: Tree search O(log n)
-- Uses idx_email, finds row instantly
```

**Best practices:**
- Index frequently queried columns (WHERE, JOIN, ORDER BY)
- Composite indexes: column order matters (most selective first)
- Avoid over-indexing (writes slower, storage overhead)
- Monitor query performance (EXPLAIN ANALYZE)
- Update statistics regularly

---

## Вопрос 33

**Database Sharding**

Sharding — это horizontal partitioning где database split across multiple servers (shards), каждый shard содержит subset of data based на shard key; enables horizontal scaling beyond single server capacity, distributes load, но добавляет complexity: cross-shard queries expensive, resharding difficult, choose shard key carefully (balanced distribution, avoid hotspots).

**Sharding strategies:**

**Hash-based sharding:**
```
Shard = hash(user_id) % num_shards

user_id=1 → hash(1) % 4 = 1 → Shard 1
user_id=2 → hash(2) % 4 = 2 → Shard 2
user_id=3 → hash(3) % 4 = 3 → Shard 3
user_id=4 → hash(4) % 4 = 0 → Shard 0

Pros: Uniform distribution
Cons: Resharding hard (changes hash mapping)
```

**Range-based sharding:**
```
Shard 1: user_id 1-1,000,000
Shard 2: user_id 1,000,001-2,000,000
Shard 3: user_id 2,000,001-3,000,000

Pros: Simple, range queries efficient
Cons: Unbalanced (hotspots if data skewed)
```

**Geographic sharding:**
```
Shard 1: users in US
Shard 2: users in Europe
Shard 3: users in Asia

Pros: Low latency (data close to users)
Cons: Unbalanced, complex routing
```

**Implementation:**
```
@Service
public class ShardedUserRepository {
    private final List<DataSource> shards;
    
    public User findById(Long userId) {
        int shardIndex = Math.abs(userId.hashCode() % shards.size());
        DataSource shard = shards.get(shardIndex);
        return queryUserFromShard(shard, userId);
    }
    
    // Cross-shard query (expensive!)
    public List<User> findByName(String name) {
        return shards.parallelStream()
            .map(shard -> queryUsersByName(shard, name))
            .flatMap(List::stream)
            .collect(Collectors.toList());
    }
}
```

**Challenges:**
- Cross-shard joins (avoid through denormalization)
- Distributed transactions (use Saga)
- Resharding (add/remove shards)
- Hotspots (celebrity users, trending data)
- Data consistency

---

## Вопрос 34-45

**(Краткие ответы)**

**34. Partitioning:** Vertical (split table columns) или horizontal (split rows), within single database, simpler than sharding.

**35. Replication:** Copy data to multiple servers, Master-Slave (writes to master, reads from replicas), async или sync, improves read scalability и availability.

**36. Master-Slave Replication:** Single master (writes), multiple slaves (reads), async replication, read scaling, slave promotion при master failure.

**37. Master-Master Replication:** Multiple masters (all accept writes), conflict resolution needed, active-active setup, higher write availability.

**38. Denormalization:** Duplicate data to avoid joins, improves read performance, increases storage, data consistency challenge, cache invalidation needed.

**39. Connection Pooling:** Reuse database connections, avoid connection overhead, HikariCP (Java), limit max connections, improves throughput.

**40. N+1 Query Problem:** 1 query для list + N queries для each item details, solve: eager loading (JOIN), batch queries, caching.

**41. Read Replicas:** Duplicate database для read-only queries, offload master, eventual consistency, monitor replication lag.

**42. Write-Heavy vs Read-Heavy:** Write-heavy: sharding, write-optimized storage (Cassandra); Read-heavy: caching, read replicas, CDN.

**43. Eventually Consistent Systems:** Temporary inconsistencies допустимы, replicas converge over time, high availability, conflict resolution strategies.

**44. Distributed Transactions:** Координация transactions across multiple databases, 2PC (blocking) или Saga (compensating), avoid когда possible.

**45. Migration Strategies:** Blue-green deployment, rolling migration, dual-write (old+new), shadow mode, feature flags, gradual cutover.

---

## Вопрос 46

**Caching Strategies**

Caching хранит frequently accessed data in fast storage (RAM) для reduced latency и decreased backend load; strategies: Cache-Aside (lazy load on cache miss), Write-Through (write to cache+DB synchronously), Write-Behind (write to cache, async to DB), Read-Through (cache auto-loads from DB); choose based на read/write ratio, consistency requirements, acceptable staleness.

**Levels:**
```
1. Client-side cache (browser, mobile app)
2. CDN cache (static content globally)
3. Application cache (in-memory, Redis)
4. Database cache (query result cache)
```

**Cache-Aside (Lazy Loading):**
```
public Product getProduct(Long id) {
    // Try cache first
    Product cached = cache.get("product:" + id);
    if (cached != null) {
        return cached; // Cache hit
    }
    
    // Cache miss: load from DB
    Product product = productRepository.findById(id);
    
    // Store in cache
    cache.set("product:" + id, product, TTL);
    
    return product;
}
```

**Write-Through:**
```
public void updateProduct(Product product) {
    // Write to cache first
    cache.set("product:" + product.getId(), product);
    
    // Then write to DB (synchronous)
    productRepository.save(product);
    
    // Always consistent: cache mirrors DB
}
```

**Write-Behind (Write-Back):**
```
public void updateProduct(Product product) {
    // Write to cache immediately
    cache.set("product:" + product.getId(), product);
    
    // Schedule async write to DB
    queue.add(new WriteTask(product));
    
    // Fast response, eventual consistency
}
```

---

## Вопрос 47-60

**(Краткие ответы продолжение)**

**47. Cache-Aside:** Application manages cache, lazy loading, cache miss loads from DB, manual invalidation, flexible но boilerplate code.

**48. Write-Through:** Write to cache+DB synchronously, always consistent, higher write latency, no stale data.

**49. Write-Behind:** Write to cache immediately, async DB write, fast writes, risk data loss если cache crashes, eventual consistency.

**50. Cache Invalidation:** TTL (time-based expiry), event-based (invalidate on update), LRU eviction, hardest problem в computer science.

**51. Redis vs Memcached:** Redis: data structures (list, set, hash), persistence, pub/sub, Lua scripts; Memcached: simple key-value, faster для pure caching, no persistence.

**52. CDN:** Content Delivery Network, caches static content (images, CSS, JS) globally, reduces latency, examples: CloudFront, Cloudflare, Akamai.

**53. Cache Eviction:** LRU (Least Recently Used), LFU (Least Frequently Used), FIFO, Random, TTL-based expiration.

**54. Cache Stampede:** Multiple requests miss cache simultaneously, overwhelm DB loading same data, solutions: locking, early expiration, stale-while-revalidate.

**55. HTTP Caching:** Browser/CDN caching через headers (Cache-Control, ETag), 304 Not Modified, reduces server load.

**56. Load Balancing Algorithms:** Round Robin, Least Connections, IP Hash, Weighted, Least Response Time, Random.

**57. Rate Limiting:** Limit requests per user/IP, algorithms: Token Bucket, Leaky Bucket, Fixed Window, Sliding Window, protects от abuse.

**58. Throttling vs Rate Limiting:** Rate Limiting - hard limit (reject excess); Throttling - delay/queue requests (slower response).

**59. Message Queues:** Async communication, decouple producers/consumers, examples: Kafka (high-throughput log), RabbitMQ (traditional queue), SQS (managed).

**60. Asynchronous Processing:** Non-blocking operations, background jobs, event-driven, improves responsiveness, eventual consistency.

---

## Вопрос 61

**Design URL Shortener (TinyURL)**

**Requirements:**
- Functional: Generate short URL from long URL, redirect short→long, custom aliases optional, expiration support
- Non-functional: High availability, low latency (<100ms), 1M new URLs/day, 100:1 read/write ratio

**High-level design:**
```
Client
  ↓
Load Balancer
  ↓
API Servers (stateless)
  ↓ ↓ ↓
Cache (Redis) ←→ Database (sharded)
  ↓
ID Generator (distributed, unique IDs)
```

**Database schema:**
```
CREATE TABLE urls (
  id BIGINT PRIMARY KEY,
  short_code VARCHAR(10) UNIQUE,
  long_url TEXT,
  user_id BIGINT,
  created_at TIMESTAMP,
  expires_at TIMESTAMP,
  clicks BIGINT DEFAULT 0,
  INDEX idx_short_code (short_code)
);
```

**Encoding algorithm:**
```
// Base62 encoding (0-9, a-z, A-Z)
public String generateShortCode(Long id) {
    String base62 = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    StringBuilder result = new StringBuilder();
    
    while (id > 0) {
        result.append(base62.charAt((int)(id % 62)));
        id /= 62;
    }
    
    return result.reverse().toString(); // e.g., "aB3xK9"
}

// ID=125 → "21" (7 chars ≈ 3.5 trillion URLs)
```

**API:**
```
// Create short URL
POST /api/shorten
{
  "longUrl": "https://example.com/very/long/url",
  "customAlias": "mylink", // optional
  "expiresAt": "2025-12-31" // optional
}

Response: { "shortUrl": "https://short.ly/aB3xK9" }

// Redirect
GET /aB3xK9
→ 301 Redirect to longUrl
```

**Caching strategy:**
```
Cache popular URLs (80/20 rule)
Cache-Aside: check cache → DB on miss
TTL: 24 hours
Eviction: LRU
```

**Scalability:**
- Sharding: hash(short_code) % shards
- Read replicas: 100:1 ratio
- CDN: не применимо (dynamic redirects)

---

## Вопрос 62

**Design Social Media Feed (Twitter/Instagram)**

**Requirements:**
- Functional: Post tweets, follow users, timeline (tweets from followed users), trending topics
- Non-functional: 100M DAU, 10K tweets/sec, timeline load <200ms, eventual consistency OK

**High-level design:**
```
Client
  ↓
Load Balancer / CDN
  ↓
API Gateway
  ↓ ↓ ↓ ↓
User Service | Tweet Service | Timeline Service | Trend Service
  ↓              ↓                 ↓                ↓
User DB      Tweet DB         Timeline Cache    Analytics DB
             (Cassandra)       (Redis)
```

**Data models:**
```
-- Users (sharded by user_id)
users: { id, username, bio, followers_count, following_count }

-- Tweets (sharded by tweet_id, replicated)
tweets: { id, user_id, content, created_at, likes, retweets }

-- Follower graph (sharded by follower_id)
follows: { follower_id, followee_id, created_at }

-- Timeline (Redis sorted set)
timeline:{user_id} → [(tweet_id, timestamp), ...]
```

**Tweet creation:**
```
public void postTweet(Long userId, String content) {
    // 1. Store tweet
    Tweet tweet = new Tweet(generateId(), userId, content, now());
    tweetRepository.save(tweet); // Cassandra
    
    // 2. Fan-out to followers' timelines (async)
    List<Long> followers = getFollowers(userId);
    for (Long followerId : followers) {
        messageQueue.send(new TimelineUpdateEvent(followerId, tweet.getId()));
    }
}

// Timeline worker (consumes queue)
public void updateTimeline(Long userId, Long tweetId) {
    redis.zadd("timeline:" + userId, timestamp, tweetId);
    redis.zremrangeByRank("timeline:" + userId, 0, -1001); // Keep latest 1000
}
```

**Timeline loading:**
```
public List<Tweet> getTimeline(Long userId, int limit) {
    // 1. Get tweet IDs from Redis
    Set<Long> tweetIds = redis.zrevrange("timeline:" + userId, 0, limit - 1);
    
    // 2. Hydrate tweets (batch query)
    return tweetRepository.findByIds(tweetIds);
}
```

**Fan-out strategies:**
- Fan-out on write (push): pre-compute timelines, fast reads, slow writes, high storage
- Fan-out on read (pull): compute timeline on request, slow reads, fast writes
- Hybrid: celebrities (pull), normal users (push)

---

## Вопрос 63

**Design Chat Application (WhatsApp/Slack)**

**Requirements:**
- Functional: 1-on-1 messaging, group chat, online/offline status, message history, read receipts
- Non-functional: Real-time delivery (<1s), 1B users, 100B messages/day, high availability

**High-level design:**
```
Client (WebSocket)
  ↓
WebSocket Gateway (stateful)
  ↓
Message Queue (Kafka)
  ↓ ↓ ↓
Chat Service | Presence Service | Notification Service
  ↓              ↓                   ↓
Message DB   User Cache          Push Service
(Cassandra)  (Redis)             (FCM/APNs)
```

**WebSocket connection:**
```
@ServerEndpoint("/ws/chat")
public class ChatWebSocket {
    private static Map<Long, Session> sessions = new ConcurrentHashMap<>();
    
    @OnOpen
    public void onOpen(Session session, @PathParam("userId") Long userId) {
        sessions.put(userId, session);
        presenceService.setOnline(userId);
    }
    
    @OnMessage
    public void onMessage(String message, Session session) {
        ChatMessage msg = parseMessage(message);
        
        // Persist message
        messageRepository.save(msg);
        
        // Deliver to recipient
        Session recipientSession = sessions.get(msg.getRecipientId());
        if (recipientSession != null && recipientSession.isOpen()) {
            recipientSession.getBasicRemote().sendText(message); // Real-time
        } else {
            // Offline: send push notification
            pushNotificationService.send(msg.getRecipientId(), msg);
        }
    }
}
```

**Message storage:**
```
-- Cassandra (partition by chat_id, sorted by timestamp)
CREATE TABLE messages (
  chat_id UUID,
  timestamp TIMESTAMP,
  message_id UUID,
  sender_id BIGINT,
  content TEXT,
  PRIMARY KEY (chat_id, timestamp, message_id)
) WITH CLUSTERING ORDER BY (timestamp DESC);
```

**Group chat:**
```
public void sendGroupMessage(Long groupId, ChatMessage message) {
    // 1. Save message
    messageRepository.save(message);
    
    // 2. Get group members
    List<Long> members = groupService.getMembers(groupId);
    
    // 3. Broadcast to online members
    members.parallelStream().forEach(memberId -> {
        Session session = sessions.get(memberId);
        if (session != null && session.isOpen()) {
            session.sendMessage(message);
        }
    });
}
```

**Presence (online/offline):**
```
// Redis with TTL
public void setOnline(Long userId) {
    redis.setex("presence:" + userId, 300, "online"); // 5 min TTL
}

// Heartbeat every 1 minute
scheduledExecutor.scheduleAtFixedRate(() -> {
    redis.expire("presence:" + userId, 300); // Refresh TTL
}, 0, 60, TimeUnit.SECONDS);
```

---

## Вопрос 64

**Monitoring и Observability**

Monitoring собирает metrics (CPU, memory, request rate, latency, errors), alerting при thresholds; Observability понимает internal state through external outputs: metrics (quantitative), logs (events), traces (request flow); включает dashboards (Grafana), APM (Application Performance Monitoring), distributed tracing (Jaeger), log aggregation (ELK), synthetic monitoring, SLIs/SLOs/SLAs.

**Three Pillars:**

**Metrics:**
```
System metrics: CPU, memory, disk, network
Application metrics: request rate, latency, error rate
Business metrics: signups, purchases, revenue

Tools: Prometheus, Grafana, Datadog, New Relic

Example:
http_requests_total{method="GET", status="200"} 10543
http_request_duration_seconds{quantile="0.99"} 0.15
```

**Logs:**
```
Structured logging с context
Centralized aggregation
Search and analysis

Tools: ELK Stack (Elasticsearch, Logstash, Kibana), Loki

Example:
{
  "timestamp": "2025-10-21T01:00:00Z",
  "level": "ERROR",
  "service": "order-service",
  "trace_id": "abc123",
  "message": "Payment failed",
  "user_id": 12345,
  "error": "InsufficientFunds"
}
```

**Traces:**
```
Distributed request tracing across microservices
Visualize latency bottlenecks
Correlate logs/metrics

Tools: Jaeger, Zipkin, AWS X-Ray

Example:
Request → API Gateway (5ms)
        → Auth Service (10ms)
        → Order Service (50ms)
          → Database (40ms)
          → Payment Service (100ms)
Total: 165ms (Payment bottleneck identified!)
```

**SLIs/SLOs/SLAs:**
```
SLI (Service Level Indicator): метрика (e.g., request latency)
SLO (Service Level Objective): цель (e.g., 99.9% requests < 200ms)
SLA (Service Level Agreement): contract с penalties

Example:
SLI: p99 latency
SLO: p99 < 200ms для 99.9% requests
SLA: Uptime 99.95% или refund
```

---

## Вопрос 65

**System Design Interview Process**

System Design Interview оценивает способность проектировать scalable distributed systems, problem-solving approach, trade-off decisions, communication skills; длительность 45-60 минут: requirements clarification (5-10 min), high-level design (15-20 min), deep dive (20-30 min), discussion trade-offs и bottlenecks; не существует "correct" solution, важен процесс reasoning и обоснование решений.

**Этапы:**

**1. Requirements Clarification (5-10 min):**
```
Вопросы interviewer:
- Who are users? (consumers, businesses, admins)
- What features needed? (MVP vs future)
- Scale expectations? (DAU, QPS, data volume)
- Performance requirements? (latency, throughput)
- Availability requirements? (99.9%, 99.99%)
- Consistency requirements? (strong vs eventual)
- Budget constraints?

Пример:
"For URL shortener:
- 100M DAU
- 1M new URLs/day
- 100:1 read/write ratio
- Latency < 100ms
- 99.9% availability
- Eventual consistency OK"
```

**2. High-Level Design (15-20 min):**
```
Draw architecture diagram:
- Major components (client, LB, API, DB, cache)
- Data flow arrows
- APIs (REST endpoints)
- Database choice (SQL vs NoSQL)

Explain:
- Why each component needed
- How components interact
- What protocols used
```

**3. Deep Dive (20-30 min):**
```
Interviewer picks areas:
- Database schema
- Scaling strategy (sharding, replication)
- Caching layer
- Security (auth, rate limiting)
- Monitoring

Показать:
- Algorithms (URL encoding, consistent hashing)
- Data models (tables, indexes)
- Trade-offs discussed
```

**4. Bottlenecks & Trade-offs (10-15 min):**
```
Identify:
- Single points of failure → add redundancy
- Performance bottlenecks → caching, sharding
- Security concerns → authentication, encryption
- Cost optimizations → right-sizing, autoscaling

Discuss alternatives:
"We chose Cassandra for high write throughput,
but PostgreSQL with sharding could work too.
Trade-off: Cassandra eventual consistency vs
PostgreSQL strong consistency."
```

**Tips:**
- Start broad, then dive deep
- Think aloud (communicate reasoning)
- Ask clarifying questions (don't assume)
- Discuss trade-offs (no perfect solution)
- Use whiteboard effectively
- Consider real-world constraints (cost, time, team size)
- Practice common designs (URL shortener, Twitter, Instagram, Uber)

**Common patterns:**
- Load Balancer для scalability
- Cache для performance
- Async processing для decoupling
- Sharding для large data
- Replication для availability
- CDN для static content

**Red flags:**
- Assuming requirements (ask!)
- Over-engineering (KISS principle)
- Ignoring constraints (scale, budget)
- Not discussing trade-offs
- Poor communication

# System Design Interview Notebook (Mid to Senior Level)

> **100 Essential System Design Questions & Answers**

---

## 1. Core Fundamentals & Concepts

### Q1. What is the difference between Horizontal and Vertical Scaling?
**Answer:** Vertical scaling (scaling up) involves adding more power (CPU, RAM) to an existing single machine. Horizontal scaling (scaling out) involves adding more machines to your pool of resources to share the load.

### Q2. What are the limitations of Vertical Scaling?
**Answer:** It has a hard hardware limit, causes downtime during upgrades, does not provide high availability (creates a Single Point of Failure), and costs increase exponentially for high-end hardware.

### Q3. What is the CAP Theorem?
**Answer:** It states that a distributed data store can only simultaneously provide two of the following three guarantees: Consistency (every read receives the most recent write), Availability (every request receives a non-error response), and Partition Tolerance (the system continues to operate despite network partitions).

### Q4. Since network partitions are inevitable, what are your actual choices in CAP?
**Answer:** Because Partition Tolerance (P) is a physical reality in distributed systems, you must choose between CP (Consistency + Partition Tolerance) or AP (Availability + Partition Tolerance) when a network failure occurs.

### Q5. What is the PACELC Theorem?
**Answer:** An extension of CAP. It states: in case of Partition (P), choose Availability (A) or Consistency (C). Else (E), when the system is running normally, choose Latency (L) or Consistency (C).

### Q6. What is the difference between Latency and Throughput?
**Answer:** Latency is the time it takes for a single request to complete (e.g., 50ms). Throughput is the number of requests a system can handle over a specific period (e.g., 10,000 requests per second).

### Q7. What does "High Availability" (HA) mean?
**Answer:** A system designed to operate continuously without failing for a designated period. It is often measured in "nines" (e.g., 99.99% availability means about 52.6 minutes of allowed downtime per year).

### Q8. What is a Single Point of Failure (SPOF)?
**Answer:** A part of a system that, if it fails, will stop the entire system from working. System design eliminates SPOFs through redundancy and replication.

### Q9. What is the difference between Stateful and Stateless architecture?
**Answer:** A stateful architecture remembers client data (session) from one request to the next on the server. A stateless architecture treats every request as an independent transaction, requiring the client to pass all necessary context (e.g., JWT tokens).

### Q10. Why are stateless services preferred in distributed systems?
**Answer:** Because they are trivially easy to scale horizontally. Any request can be routed to any server instance since no local session data is required to process it.

### Q11. What is Eventual Consistency?
**Answer:** A consistency model used in distributed systems (often AP systems) guaranteeing that, if no new updates are made, all replicas will eventually converge to the same value.

### Q12. What is Strong Consistency?
**Answer:** A model where any read operation is guaranteed to return the value of the most recent write operation. Usually involves locking and impacts latency/availability.

### Q13. What is a SLA, SLO, and SLI?
**Answer:** SLA (Service Level Agreement) is the legal contract with customers. SLO (Service Level Objective) is the internal target goal (e.g., 99.9% uptime). SLI (Service Level Indicator) is the actual real-time measurement (e.g., 99.95% uptime achieved).

### Q14. What is Backpressure?
**Answer:** A mechanism where a downstream system signals an upstream system to slow down its data transmission rate because the downstream system is overloaded.

### Q15. What is Graceful Degradation?
**Answer:** Designing a system so that when a component fails or is under heavy load, it falls back to a simpler, less resource-intensive function rather than crashing entirely (e.g., hiding recommendations but still allowing checkout).

### Q16. What is Disaster Recovery?
**Answer:** A set of policies and procedures to enable the recovery or continuation of vital technology infrastructure and systems following a natural or human-induced disaster.

### Q17. What is RPO and RTO?
**Answer:** RPO (Recovery Point Objective) is the maximum acceptable amount of data loss measured in time. RTO (Recovery Time Objective) is the maximum acceptable time a system can be offline before being restored.

### Q18. What is the Thundering Herd Problem?
**Answer:** Occurs when a large number of processes or threads waiting for an event are awoken simultaneously, exhausting system resources as they all try to handle the event or query a database at once.

### Q19. What is Idempotency?
**Answer:** A property of an operation where applying it multiple times yields the same result as applying it once. Crucial in distributed systems for safe retries (e.g., charging a credit card with an idempotency key).

### Q20. What is a Reverse Proxy?
**Answer:** A server that sits in front of backend servers and intercepts client requests. It provides security, SSL termination, load balancing, and caching.

---

## 2. Load Balancing, Networking & APIs

### Q21. What is a Load Balancer?
**Answer:** A device or software that distributes incoming network traffic across a group of backend servers to ensure no single server becomes overwhelmed, improving responsiveness and availability.

### Q22. What is the difference between Layer 4 and Layer 7 Load Balancing?
**Answer:** Layer 4 (Transport layer) routes traffic based on IP and Port, without inspecting the payload (fast, blind). Layer 7 (Application layer) routes traffic based on HTTP headers, URLs, or cookies (slower, smart).

### Q23. Name common Load Balancing Algorithms.
**Answer:** Round Robin, Least Connections, IP Hash, Weighted Round Robin, and Least Response Time.

### Q24. What is Consistent Hashing?
**Answer:** A distributed hashing scheme that operates independently of the number of servers. When a server is added or removed, it minimizes data reorganization (only `K/n` keys are remapped), making it essential for distributed caches and databases.

### Q25. How do you handle "Hotspots" in Consistent Hashing?
**Answer:** By using "Virtual Nodes". Each physical server is assigned multiple points on the hash ring, ensuring a more uniform distribution of keys.

### Q26. What is DNS (Domain Name System)?
**Answer:** The "phonebook of the internet." It translates human-readable domain names (www.google.com) into machine-readable IP addresses.

### Q27. What is an API Gateway?
**Answer:** A single entry point for all clients into a microservices architecture. It handles routing, composition, rate limiting, authentication, logging, and SSL termination.

### Q28. API Gateway vs. Load Balancer?
**Answer:** A load balancer primarily distributes traffic across identical servers. An API gateway routes requests to *different* specific microservices based on the URL and handles cross-cutting concerns like auth and rate limiting.

### Q29. REST vs. GraphQL?
**Answer:** REST exposes multiple endpoints for different resources and can suffer from over-fetching/under-fetching. GraphQL exposes a single endpoint and allows the client to query exactly the specific data schema they need.

### Q30. REST vs. gRPC?
**Answer:** REST uses HTTP/1.1 and JSON (human-readable, slower, larger payloads). gRPC uses HTTP/2 and Protocol Buffers (binary, compressed, strictly typed, extremely fast), making it ideal for internal microservice-to-microservice communication.

### Q31. What is WebSockets?
**Answer:** A protocol providing full-duplex, continuous bi-directional communication over a single TCP connection. Best for real-time apps like chat, gaming, or trading platforms.

### Q32. What is Server-Sent Events (SSE)?
**Answer:** A unidirectional protocol where the client establishes a connection and the server streams data updates to the client. Best for news feeds, stock tickers, or live scores where the client rarely sends data back.

### Q33. What is Long Polling?
**Answer:** A technique where the client requests information from the server, and the server holds the request open until new data is available. Once the server responds, the client immediately sends a new request.

### Q34. What is Rate Limiting?
**Answer:** Controlling the rate of traffic sent or received by a network to prevent DoS attacks, brute force, and server overload.

### Q35. Explain the Token Bucket algorithm.
**Answer:** A bucket holds a maximum number of tokens. Tokens are added at a constant rate. Each request costs one token. If the bucket is empty, the request is dropped. It allows for short bursts of traffic.

### Q36. Explain the Leaky Bucket algorithm.
**Answer:** Requests are added to a queue (bucket) and processed (leaked) at a constant, fixed rate. It smooths out bursty traffic into a steady stream. If the queue is full, new requests are dropped.

### Q37. What is a CDN (Content Delivery Network)?
**Answer:** A globally distributed network of proxy servers deployed in multiple data centers. It serves static assets (images, JS, CSS, videos) to users from the server geographically closest to them, minimizing latency.

### Q38. CDN Push vs. CDN Pull?
**Answer:** Pull CDN: The CDN grabs the content from the origin server when a user requests it for the first time. Push CDN: The engineering team proactively uploads the content to the CDN before users request it.

### Q39. What is Anycast Routing?
**Answer:** A network addressing and routing methodology where a single IP address is shared by multiple servers in different locations. The network routes the request to the topologically closest server. Often used by CDNs and DNS.

### Q40. What is TLS/SSL Termination?
**Answer:** The process of decrypting encrypted traffic at the load balancer or API Gateway, allowing the internal network traffic to be sent unencrypted, which saves CPU cycles on the backend servers.

---

## 3. Databases & Data Storage

### Q41. Relational (SQL) vs. Non-Relational (NoSQL) databases?
**Answer:** SQL databases are table-based, enforce strict schemas, and use ACID transactions (e.g., Postgres, MySQL). NoSQL databases are document, key-value, graph, or wide-column stores, have flexible schemas, scale horizontally easily, and usually favor eventual consistency (e.g., MongoDB, Cassandra).

### Q42. Name the 4 main types of NoSQL databases.
**Answer:** 1. Key-Value (Redis, DynamoDB), 2. Document (MongoDB, CouchDB), 3. Wide-Column/Column-Family (Cassandra, HBase), 4. Graph (Neo4j).

### Q43. When would you choose Cassandra over MySQL?
**Answer:** When you need massive, highly available horizontal scalability, masterless architecture, and heavily prioritize write-throughput over complex JOINs and ACID transactions.

### Q44. What is Database Sharding?
**Answer:** A horizontal partitioning technique where a large database is divided into smaller, faster, more easily managed parts called shards, which are distributed across multiple independent servers.

### Q45. What are the challenges of Sharding?
**Answer:** Complex JOINs across shards are nearly impossible, resharding (adding new nodes) is difficult, and hot keys (celebrity problem) can overload a single shard.

### Q46. What is a Shard Key (Partition Key)?
**Answer:** The specific column used to distribute data across shards. Choosing a good shard key (e.g., `user_id`, `hash(id)`) is critical to evenly distributing read/write loads.

### Q47. What is Database Replication?
**Answer:** Keeping copies of a database on multiple machines to ensure high availability and improve read throughput.

### Q48. Master-Slave vs. Master-Master replication?
**Answer:** Master-Slave: One node handles writes, multiple nodes handle reads. Master-Master: Multiple nodes can accept writes, requiring complex conflict resolution (e.g., last-write-wins).

### Q49. Synchronous vs. Asynchronous Replication?
**Answer:** Sync: The master waits for replicas to confirm the write before responding to the client (high consistency, slow). Async: The master responds immediately and replicates in the background (fast, risk of data loss if master dies).

### Q50. What is the Leader Election process?
**Answer:** In a distributed system, if the master node goes down, the remaining replica nodes must communicate to agree upon (elect) a new master to avoid split-brain scenarios. Algorithms like Paxos and Raft handle this.

### Q51. What is the Split-Brain problem?
**Answer:** A state where a network partition causes two nodes to both believe they are the Master, causing them to accept conflicting writes, leading to severe data corruption.

### Q52. What is a Distributed Transaction?
**Answer:** A database transaction in which two or more network hosts are involved. It guarantees ACID properties across multiple independent databases or microservices.

### Q53. Explain the Two-Phase Commit (2PC) protocol.
**Answer:** A distributed transaction protocol. Phase 1 (Prepare): The coordinator asks all nodes if they are ready to commit. Phase 2 (Commit): If all say yes, the coordinator tells them to commit. If any say no, it tells all to abort. It is synchronous and blocking.

### Q54. What is the CAP theorem implication for Cassandra vs. HBase?
**Answer:** Cassandra is an AP system (favors Availability and Partition tolerance, uses eventual consistency). HBase is a CP system (favors Consistency and Partition tolerance).

### Q55. What is a Wide-Column Store good for?
**Answer:** Highly scalable, distributed storage capable of handling massive amounts of data across commodity servers. Excellent for time-series data, IoT sensor data, and heavy write workloads.

### Q56. What is Object Storage vs. Block Storage?
**Answer:** Block Storage (like AWS EBS) splits data into raw blocks, attached to an OS like a hard drive. Object Storage (like AWS S3) stores files as complete objects with metadata and a unique URL, highly scalable for media/backups.

### Q57. What is Data Partitioning?
**Answer:** Dividing a large database table into multiple smaller parts within the same database server to improve query performance and maintenance (e.g., partitioning a logs table by month).

### Q58. What is a Vector Database?
**Answer:** A database designed to store, manage, and search high-dimensional vectors. Crucial for AI applications, recommendation systems, and semantic search (e.g., Pinecone, Milvus).

### Q59. How would you design a database schema for a Chat application?
**Answer:** Typically a NoSQL Wide-Column store like Cassandra for messages to handle immense write volume, partitioned by `chat_id`. A relational DB could be used for user profiles and settings.

### Q60. What is a Bloom Filter?
**Answer:** A highly space-efficient probabilistic data structure used to test whether an element is a member of a set. It can definitively tell you if a key is *not* in the database, preventing unnecessary disk lookups.

---

## 4. Caching Systems

### Q61. What is the primary purpose of a Cache?
**Answer:** To store copies of frequently accessed, expensive-to-compute, or slow-to-retrieve data in fast memory (RAM) to dramatically reduce latency and backend load.

### Q62. Cache Aside (Lazy Loading) Strategy?
**Answer:** The application checks the cache first. If a miss occurs, it queries the database, saves the result in the cache, and returns it. Pros: Cache only contains requested data. Cons: Cache miss causes latency.

### Q63. Write-Through Cache Strategy?
**Answer:** The application writes data to the cache and the database simultaneously. Pros: Cache is always up-to-date. Cons: Write latency is higher since it involves two operations.

### Q64. Write-Behind (Write-Back) Cache Strategy?
**Answer:** The application writes data only to the cache, returning immediately. An async background process writes the data to the database. Pros: Extremely fast writes. Cons: Risk of data loss if the cache crashes before DB sync.

### Q65. What is Cache Eviction?
**Answer:** The process of removing items from the cache when it reaches its size limit.

### Q66. What is LRU (Least Recently Used)?
**Answer:** An eviction policy that removes the item that hasn't been accessed for the longest time, assuming recently accessed items will likely be accessed again soon.

### Q67. What is LFU (Least Frequently Used)?
**Answer:** An eviction policy that tracks how often an item is accessed and evicts the item with the lowest access count.

### Q68. What is Cache Penetration?
**Answer:** When malicious users repeatedly request a key that does not exist in the cache *or* the database. The cache keeps missing, forcing the database to be hammered. Solved using Bloom Filters or caching empty results.

### Q69. What is Cache Breakdown?
**Answer:** When a highly requested "hot key" expires in the cache, and massive concurrent requests immediately hammer the database to recompute it. Solved by adding mutual exclusion locks (mutex) to database re-computation.

### Q70. What is Cache Avalanche?
**Answer:** When a massive number of cached items expire at the exact same time, causing a sudden spike in database traffic. Solved by adding random jitter (randomized time) to cache expiration TTLs (Time to Live).

### Q71. Redis vs. Memcached?
**Answer:** Memcached is a simple, volatile, multi-threaded key-value store. Redis is single-threaded but supports advanced data structures (Lists, Sets, Hashes, Geospatial), persistence to disk, replication, and pub/sub.

### Q72. How does Redis achieve high performance despite being single-threaded?
**Answer:** Redis is entirely in-memory and uses non-blocking I/O multiplexing (epoll). Since memory operations are incredibly fast, avoiding thread context-switching and lock contention actually improves throughput.

### Q73. What is Redis Persistence (RDB vs AOF)?
**Answer:** RDB (Redis Database) takes periodic snapshots of the dataset. AOF (Append Only File) logs every write operation. AOF provides better durability but larger files; RDB is faster for recovery.

### Q74. What is a Distributed Cache?
**Answer:** A cache scaled across multiple nodes using consistent hashing (e.g., Redis Cluster). It provides massive memory capacity and high availability.

### Q75. What is HTTP ETag?
**Answer:** An HTTP header used for web cache validation. The server sends a hash of the resource. On subsequent requests, the client sends the ETag; if the resource hasn't changed, the server replies with a lightweight `304 Not Modified`.

### Q76. How would you cache user session data?
**Answer:** Using a distributed in-memory store like Redis. The application passes a session ID cookie, which acts as the key to retrieve the session object from Redis.

### Q77. What is TTL?
**Answer:** Time-To-Live. An integer value dictating how long a cached item or network packet should remain valid before it is automatically deleted.

### Q78. What happens when Redis runs out of memory?
**Answer:** Depending on the `maxmemory-policy` configuration, it will either return errors on write commands (`noeviction`) or start evicting keys based on LRU, LFU, or random policies.

### Q79. Why not cache everything?
**Answer:** RAM is highly expensive compared to disk storage. Additionally, caching highly volatile data leads to constant cache invalidation and sync issues, negating the performance benefits.

### Q80. How do you implement a Global Counter (e.g., YouTube views) at scale?
**Answer:** Do not update a relational DB directly. Use Redis `INCR` for real-time fast counting, and an async background job (Write-Behind) to periodically flush the accumulated counts to the persistent database.

---

## 5. Message Queues, Event-Driven & Microservices

### Q81. What is the difference between a Message Queue and Pub/Sub?
**Answer:** In a Message Queue (Point-to-Point), a message is consumed by only *one* consumer (e.g., processing an image). In Pub/Sub (Publish/Subscribe), a message is broadcast to *all* active subscribers (e.g., notifying multiple microservices of a user registration).

### Q82. RabbitMQ vs. Apache Kafka?
**Answer:** RabbitMQ is a traditional message broker focusing on complex routing and message acknowledgment. Kafka is a distributed event streaming platform built as an append-only log, focusing on high throughput, message retention, and stream processing.

### Q83. How does Kafka manage consumer scaling?
**Answer:** Through Partitions and Consumer Groups. A topic is split into partitions. A Consumer Group can have multiple consumers, and Kafka assigns each partition to exactly one consumer in the group, parallelizing the workload.

### Q84. What is Dead Letter Queue (DLQ)?
**Answer:** A specialized queue where messages are routed if they cannot be processed successfully after a certain number of retries. It allows engineers to inspect and debug failing messages without blocking the main queue.

### Q85. Monolithic vs. Microservices Architecture?
**Answer:** A Monolith is a single, unified codebase and deployable artifact. Microservices split the application into small, loosely coupled, independently deployable services communicating over a network.

### Q86. What are the main drawbacks of Microservices?
**Answer:** Increased operational complexity, difficult distributed debugging, network latency, complex distributed transactions, and overhead of managing multiple CI/CD pipelines and infrastructure.

### Q87. What is Service Discovery?
**Answer:** A mechanism (like Consul or Eureka) that allows microservices to automatically find each other's network locations (IP/Port) dynamically as instances scale up and down, without hardcoding IP addresses.

### Q88. What is the Circuit Breaker Pattern?
**Answer:** A proxy that monitors for failures in remote service calls. If a service fails repeatedly, the circuit "trips" (opens) and returns an immediate error without attempting the call, preventing cascading failures and allowing the broken service to recover.

### Q89. What is the Bulkhead Pattern?
**Answer:** Isolating elements of an application into separate pools (like bulkheads in a ship) so that if one fails or consumes all its resources, the others continue to function (e.g., separate thread pools for different microservice clients).

### Q90. What is the Saga Pattern?
**Answer:** A sequence of local transactions used to handle distributed transactions across microservices. Each local transaction updates the database and publishes a message triggering the next step. If one step fails, executing compensating transactions rolls back the previous steps.

### Q91. Choreography vs. Orchestration in Sagas?
**Answer:** Choreography: Services react to events autonomously without a central controller. Orchestration: A central orchestrator service tells the participating services what local transactions to execute.

### Q92. What is Event Sourcing?
**Answer:** Instead of storing just the current state of data, Event Sourcing stores every state-changing event as an immutable sequence in an append-only log. The current state is derived by replaying the events.

### Q93. What is CQRS (Command Query Responsibility Segregation)?
**Answer:** An architectural pattern that separates the models used to read data (Queries) from the models used to write data (Commands), often using entirely different databases optimized for each task.

### Q94. What is Distributed Tracing?
**Answer:** A method used to profile and monitor applications built on microservices. A unique Trace ID is generated at the API Gateway and passed along in headers to every service, allowing engineers to visualize the entire request journey (e.g., using Jaeger or Zipkin).

### Q95. What is a Service Mesh?
**Answer:** A dedicated infrastructure layer (like Istio) that abstracts network communication between microservices. It handles load balancing, service discovery, encryption (mTLS), and circuit breaking via sidecar proxies, freeing developers from writing network logic.

### Q96. How do you design a URL Shortener (like bit.ly)?
**Answer:** Generate a unique integer ID for every long URL via a standalone ID generator (like Snowflake) or DB auto-increment. Convert the base-10 ID to Base62 to create the short string. Store the mapping in a highly available DB (like Cassandra or DynamoDB) and use a Redis cache for frequent lookups.

### Q97. How do you generate unique IDs in a distributed system?
**Answer:** Use an algorithm like Twitter Snowflake, which generates 64-bit integers composed of a timestamp, datacenter/machine ID, and a sequence number. This ensures IDs are globally unique, sortable by time, and highly scalable without coordination.

### Q98. How do you design a highly available system for uploading large video files?
**Answer:** Have the client request a presigned URL from the backend, then upload the file directly to object storage (AWS S3) to bypass backend network bottlenecks. S3 triggers an event to a Message Queue, which background workers pick up for video processing/transcoding.

### Q99. What is the Strangler Fig Pattern?
**Answer:** A pattern for gradually migrating a legacy monolith to microservices by replacing specific functionality piece by piece with new services. An API Gateway routes traffic to either the monolith or the new service until the monolith can be "strangled" completely.

### Q100. How do you approach a System Design Interview?
**Answer:** 1. Clarify Requirements (Functional & Non-Functional). 2. Back-of-the-envelope estimation (Storage, Bandwidth). 3. High-level design (APIs, core components). 4. Deep dive into specific components (DB schema, bottlenecks). 5. Identify trade-offs and scaling strategies (Caching, Sharding, Queues).
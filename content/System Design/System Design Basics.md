---
title: System Design Basics
tags:
    - system-design
    - basics
---

## Challenges
Challenges in scaling distributed systems are generally related to serving a huge number of [[daily active users]]--without a very large-scale user base, system design problems scale back to programming patterns.

### 1. Amount of users
Any single machine has a limit to the number of [[queries per second]]/[[requests per second]] it can handle, and performance will quickly degrade once the limit is exceeded. This challenge is alleviated by replication: [[load balancing]] (in the case of server logic), and/or [[data replication]].

### 2. Size of data
Data is considered "big" when it's no longer possible to hold it on a single machine. This problem is solved by [[sharding]], IE. partitioning/grouping data by some logic.

### 3. Responsiveness/latency
Response times should be < 500ms. Given properly implemented replication reads tend to be fast, so the problem primarily relates to write time. The solution is to return a response to the write request immediately, and put the data in a queue (using EG. a message queue like Kafka) to be processed asynchronously. 

### 4. Consistency
With data replication and asynchronous data update, read requests can sometimes see inconsistent (IE. outdated) data. The solution depends on the system and/or application in question--a social media app will tolerate [[eventual consistency]] better than EG. a banking app.

## Scaling

### Decomposition
Divide the system into into small, independent services. These [[microservices]] should each be based on specific requirements and should each have a single responsibility.

### Vertical Scaling
Use more powerful machines.

### Horizontal Scaling
Run multiple identical instances of stateless services and distribute requests using load balancing.

### Partitioning
Split requests and/or data into shards (based on EG. some logical key) and distribute them across services and/or databases.

### Caching
Store frequently accessed data in memory storage.

### Message Queues
Buffer write requests with message queues.

### Separate Read/Writes
On the system level, implement a _leader-follower_ architecture where writes occur on the leader, and followers provide read replicas.

Further, on the application level, utilise the [[command-query responsibility segregation]] pattern:
- handle create, update, delete operations using a write-optimised data model (commands)
- handle read operations using a read-optimised denormalised data model (queries)

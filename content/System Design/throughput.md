---
title: Throughput
tags:
    - system-design
    - throughput
    - non-functional-requirements
---
_Throughput_ refers to the rate at which a system is able to process data or complete operations within a given time period. It is typically measured in operations/second, bits/second, or transactions/second. It is a metric closely related to, but distinct from [[requests per second]], as two apps with the same throughput of 1000 MB/s could have different request rates, EG. 10 requests of 100MB per second vs. 1000 requests of 1MB per second. The [[access pattern]] must be taken into account when designing a system.

**Common measurements:**
    - **Network throughput:** Megabits per second (Mbps) or Gigabits per second (Gbps)
    - **Disk throughput:** Input/output operations per second (IOPS) or MB/s
    - **Application throughput:** Requests per second (RPS) or transactions per second (TPS)
    - **Database throughput:** Queries per second (QPS) or transactions per minute (TPM)
- **Factors affecting throughput:**
    - **Hardware capacity:** CPU, memory, network interface capabilities
    - **Parallelism:** Number of concurrent operations the system can handle
    - **Bottlenecks:** Slowest component in the processing pipeline
    - **Resource contention:** Competition for shared resources
    - **Data size:** Volume of data being processed per operation
- **Relationship with other metrics:**
    - Often trades off with latency (higher throughput may increase latency)
    - Directly impacts system capacity and scalability
    - Affects resource utilisation and operational costs
    - Influences user concurrency limits
- **Optimisation approaches:**
    - Horizontal scaling (adding more machines)
    - Vertical scaling (adding more resources to existing machines)
    - Load balancing to distribute work evenly
    - Caching to reduce repetitive processing
    - Batch processing to amortise overhead costs

## Increasing throughput
We can either decrease the time it takes to process a request (IE. decreasing [[latency]]), or increase the number of requests processed per unit of time (increasing [[concurrency]]).
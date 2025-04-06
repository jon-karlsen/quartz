---
title: Resource Estimation
tags:
    - system-design
    - scalability
    - availability
    - latency
    - throughput
    - requests-per-second
    - rps
    - queries-per-second
    - qps
    - transactions-per-second
    - tps
---

## Load Parameters
Load parameters describe the demands placed on a system.

- **request rate:** number of [[requests per second]] or [[queries per second]]
- **data volume:** size of data being processed or stored
- **concurrent users:** number of simultaneous users or connections
- **read/write ratio:** proportion of read operations to write operations
- **traffic patterns:** distributions of load over time, EG. steady, bursty, cyclical

## Performance Parameters
Performance parameters measure how well a system handles load:
- **[[latency]]:** time taken to process a request (average, median, percentiles)
- **[[throughput]]:** rate at which work is completed ([[requests per second]], [[transactions per second]])
- **resource utilisation:** CPU, memory, disk, network usage percentages
- **error rate:** percentage of failed requests
- **[[availability]]:** system uptime
- **[[scalability]]:** how performance changes as load increases

## Estimation

### Back-of-the-Envelope Calculation
Starting from known quantities and deriving related values, powers of 10 and simple arithmetic are used to make quick calculations that help establish order-of-magnitude estimates, a major benefit of which are early reality checks that prevent major design flaws. 

Examples:
- 10 million [[daily active users]] who on average make 5 requests a day means ~50 million requests per day, ~580 [[requests per second]]
- 1 million users each storing 100MB data necessitates ~100TB of storage capacity

### Benchmarking
Run controlled tests on current systems and/or components to gather empirical data, based on which predictions about future needs can be made. 

### Performance Modelling
Use queuing theory, statistical analysis, and/or simulation software to predict performance. This approach can help explore system behaviour under conditions that are difficult to directly test.

### Capacity Planning
Project resource requirements over time by analysing current usage patterns, growth trends, seasons variations, and so on.

### Stress Testing
Observe system behaviour under controlled load increases.

### Traffic Pattern Analysis
Examine how load varies over different time periods.

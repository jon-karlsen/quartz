---
title: Non-Functional Requirements
tags:
    - system-design
    - non-functional-requirements
---

Non-functional requirements describe how a system should behave, as well as the constraints under which it must operate. The most common non-functional requirements are:

- [[availability]]: the degree to which a system is operational and accessible when needed, typically expressed as a percentage of uptime over total time
- [[scalability]]: a system's ability to handle increased load without significant degradation of performance
- [[latency]]: after a request has been made, the delay before data transfer begins
- [[throughput]]: the amount of work/information/transactions a system can process within a given period
- [[consistency]]: ensuring that all nodes of a distributed system see the same data at the same time

## System design components to non-functional requirements

|Non-functional Requirement|Load Balancing|Caching|Partitioning|Replication|Message Queue|Batch Processing|
|---|---|---|---|---|---|---|
|Availability|X|X|X|X|X||
|Scalability|X|X|X|X|X|X|
|Latency|X|X|X|X|||
|Consistency<sup>1</sup>||X|X|X|||

<sup>1</sup> Affected by
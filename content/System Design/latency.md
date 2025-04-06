---
title: Latency
tags:
    - system-design
    - non-functional-requirements
    - latency
---

_Latency_ refers to the time delay between the initiation of a request and the receipt of a response in a system. It is measured as the total round-trip time for data to travel from source to destination, and back.

**Types of latency:**
    - **Network latency:** Time for data to travel across a network
    - **Processing latency:** Time spent computing or processing a request
    - **Storage latency:** Time to read from or write to storage devices
    - **Database latency:** Time to execute and return results from a query
    - **Application latency:** Overall delay experienced by end-users

**Measurement units:**
    - Milliseconds (ms) for most applications
    - Microseconds (μs) for high-performance systems
    - Nanoseconds (ns) for specialised low-latency environments

**Factors affecting latency:**
    - **Physical distance:** Longer distances increase network travel time
    - **Network congestion:** Traffic volume can delay packet delivery
    - **Hardware limitations:** CPU, memory, or I/O bottlenecks
    - **Software efficiency:** Code quality and architectural choices
    - **Service dependencies:** Waiting for other services or APIs

**Importance in system design:**
    - Critical for real-time applications (gaming, trading, video conferencing)
    - Directly impacts user experience and satisfaction
    - Often traded off against throughput, cost, or consistency
    - Key metric for service level agreements (SLAs)

## Tail Latency
_Tail latency_ refers to the requests experiencing the longest delays, typically measured at the 99<sup>th</sup>, 99.9<sup>th</sup>, or even 99.99<sup>th</sup> percentile of a distribution. This is an essential metric as it often correlates with the worst user experiences, the reduction of which is an important, but frequently challenging goal. Its realisation might include software optimisation, hardware upgrades, or even changing system architecture. It might also involve improving load balancing, or implementing better failover strategies to handle peak traffic or outages.

## Managing Latency
Latency can be managed through the use of [[content delivery network]]s, [[load balancing]], [[caching]], and the selection of appropriate data centre locations.
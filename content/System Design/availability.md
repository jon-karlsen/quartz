---
title: Availability
tags:
    - system-design
    - non-functional-requirements
    - availability
---

_Availability_ refers to the degree to which a system is operational and accessible when needed.

## Measuring Availability
Availability is typically expressed in terms of a percentage of uptime over total time:

```
Availability (%) = (Total operational time / Total time) x 100
```

_Total time_ refers to the total amount of time for which the system's availability is being measured, and could be one or more days, weeks, months, years, and so on.

_Downtime_ refers to the total amount of time during the period of evaluation during which the system was non-operational.

_Operational time_ is calculated by subtracting downtime from total time.

## High Availability
_High availability_ refers to the ability of a system to operate continuously, without failure, and for a significantly long period of time. High availability is often talked about in terms of "nines": a system with "five nines" of availability would have a downtime of only 5.26 minutes a year.

|Availability %|Downtime per year|Downtime per quarter|Downtime per month|Downtime per week|Downtime per day (24 hours)|
|---|---|---|---|---|---|
|90% ("one nine")|36.53 days|9.13 days|73.05 hours|16.80 hours|2.40 hours|
|95% ("one nine five")|18.26 days|4.56 days|36.53 hours|8.40 hours|1.20 hours|
|97% ("one nine seven")|10.96 days|2.74 days|21.92 hours|5.04 hours|43.20 minutes|
|98% ("one nine eight")|7.31 days|43.86 hours|14.61 hours|3.36 hours|28.80 minutes|
|99% ("two nines")|3.65 days|21.9 hours|7.31 hours|1.68 hours|14.40 minutes|
|99.5% ("two nines five")|1.83 days|10.98 hours|3.65 hours|50.40 minutes|7.20 minutes|
|99.8% ("two nines eight")|17.53 hours|4.38 hours|87.66 minutes|20.16 minutes|2.88 minutes|
|99.9% ("three nines")|8.77 hours|2.19 hours|43.83 minutes|10.08 minutes|1.44 minutes|
|99.95% ("three nines five")|4.38 hours|65.7 minutes|21.92 minutes|5.04 minutes|43.20 seconds|
|99.99% ("four nines")|52.60 minutes|13.15 minutes|4.38 minutes|1.01 minutes|8.64 seconds|
|99.995% ("four nines five")|26.30 minutes|6.57 minutes|2.19 minutes|30.24 seconds|4.32 seconds|
|99.999% ("five nines")|5.26 minutes|1.31 minutes|26.30 seconds|6.05 seconds|864.00 [milliseconds](https://en.wikipedia.org/wiki/Millisecond "Millisecond")|
|99.9999% ("six nines")|31.56 seconds|7.89 seconds|2.63 seconds|604.80 milliseconds|86.40 milliseconds|
|99.99999% ("seven nines")|3.16 seconds|0.79 seconds|262.98 milliseconds|60.48 milliseconds|8.64 milliseconds|
|99.999999% ("eight nines")|315.58 milliseconds|78.89 milliseconds|26.30 milliseconds|6.05 milliseconds|864.00 [microseconds](https://en.wikipedia.org/wiki/Microsecond "Microsecond")|
|99.9999999% ("nine nines")|31.56 milliseconds|7.89 milliseconds|2.63 milliseconds|604.80 microseconds|86.40 microseconds|
|99.99999999% ("ten nines")|3.16 milliseconds|788.40 microseconds|262.80 microseconds|60.48 microseconds|8.64 microseconds|
|99.999999999% ("eleven nines")|315.58 microseconds|78.84 microseconds|26.28 microseconds|6.05 microseconds|864.00 [nanoseconds](https://en.wikipedia.org/wiki/Nanosecond "Nanosecond")|
|99.9999999999% ("twelve nines")|31.56 microseconds|7.88 microseconds|2.63 microseconds|604.81 nanoseconds|86.40 nanoseconds|
<small>Source: [WikiPedia](https://en.wikipedia.org/wiki/High_availability)<small>

Expectations regarding service uptime are often set in a _service-level agreement_ between the service provider and the customer. Such agreements, often abbreviated SLAs, will often include specific metrics and targets (EG. system uptime, response time, resolution time) that the service provider agrees to meet.

## Achieving High Availability
High availability is fundamentally about continuing to operate without failures, which can be separated into _expected failures_ (EG. hardware failures, resource exhaustion) _unexpected failures_ (EG. malevolent actors, dependency failures, etc.).

Expected failures can be mitigated through [[redundancy]], [[load balancing]] stateless servers, [[data replication]], and/or automatic failover. Health monitoring and recovery systems are commonly used in conjunction with the above. A system that is able to continue operating correctly even in the event of partial system failures would be called a _fault-tolerant system_. Such a system would eliminate single points of failure, and be able to detect and repair faults automatically, without human intervention. Fault tolerance directly contributes to a system's high availability.
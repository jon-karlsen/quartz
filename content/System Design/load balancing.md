---
title: Load Balancing
tags:
    - system-design
    - load-balancing
    - non-functional-requirements 
---

_Load balancing_ refers to the process of distributing network traffic, computing tasks, or application requests across multiple severs or resources. The goal is to optimise resource utilisation, maximise throughput, minimise response time, and avoid overloading any single resource.

Benefits include improved application availability and reliability, scalable architecture that can handle traffic spikes, simplified maintenance with the ability to add/remove servers without downtime, and enhanced security through traffic filtering and DDoS protection.

Types of load balancers:
- **hardware load balancers:** dedicated physical devices optimised for traffic management
- **software load balancers:** applications running on standard servers or virtual machines
- **DNS load balancing:** using DNS to distribute traffic across multiple IP addresses
- **global load balancing:** distributing traffic across multiple geographic regions

Load balancing algorithms:
- **Round Robin:** requests are distributed sequentially across servers
- **Least Connections:** routes to server with fewest active connections
- **Least Response Time:** selects server with fastest response times
- **IP Hash:** uses client IP address to determine which server receives the request
- **Weighted methods:** assigns different capacities to different servers

Common features:
- health checks to detect server failures
- session persistence/sticky sessions
- SSL termination
- content-based routing
---
title: Redundancy
tags:
    - system-design
    - availability
    - redundancy
    - non-functional-requirements 
---

_Redundancy_ refers to the duplication of critical components and/or functions within a system, with the goal of ensuring continuous operation even in the case of individual components failing.

Benefits include improved availability and uptime, failure isolation, elimination of single points of failure, and support for maintenance without service interruption. Downsides are increased cost (hardware, licences, management overhead), added complexity in design and maintenance, potential for increased latency, and consistency challenges.

Types of redundancy:
- **hardware redundancy:** duplicate servers, storage systems, power supplies, network equipment
- **data redundancy:** store multiple copies of data across different storage locations
- **geographic redundancy:** distribute systems across different physical locations
- **network redundancy:** establish multiple connection paths between systems

Implementation approaches:
- **active-active:** multiple components operate simultaneously, sharing the workload
- **active-passive:** backup components remain idle until primary components fail
- **N+1, N+2:** keep one or more extra components beyond minimal requirements

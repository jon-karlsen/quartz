---
title: Data Replication
tags:
    - system-design
    - data-replication
    - non-functional-requirements 
---

_Data replication_ refers to the process of creating and maintaining multiple copies of the same data across different storage devices or locations. The goal is to ensure data availability, reliability, and accessibility.

Benefits include high [[availability]], disaster recovery, read scalability, and geographic performance. Challenges include consistency management, conflict resolution, network overhead, and replication lag.

Replication strategies:
- **Synchronous replication:** Changes are applied to all replicas before confirming transaction completion
- **Asynchronous replication:** Primary system completes transactions first, then changes are propagated to replicas
- **Semi-synchronous replication:** Confirms at least one secondary has received changes before completing transaction

Replication topologies:
- **Primary-Secondary (Master-Slave):** One primary node accepts writes, secondaries replicate data
- **Multi-Primary (Multi-Master):** Multiple nodes accept writes, changes are synchronized
- **Hierarchical:** Replicas are arranged in a tree-like structure
- **Peer-to-peer:** All nodes have equal status and can exchange updates

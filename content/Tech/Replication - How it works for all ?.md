---
title: '"Replication"'
draft: 
tags:
  - architecture
  - High-Level-Design
  - replication
  - mongodb
  - elasticsearch
  - MySQL
  - neo4j
---
# Replication in Distributed Systems: A Deep Dive

## What Is Replication and Why Is It Important?

Replication is the process of creating and maintaining multiple copies of data across different nodes. Its primary goals are:

* **Fault Tolerance**: If one node fails, another has a copy of the data.
* **High Availability**: Data is accessible even during node failures.
* **Read Scalability**: Distribute read requests across replicas to avoid overloading a single node.
* **Geographic Proximity**: Serve users closer to the data to reduce latency.

---

## Leader–Follower Replication

The most common replication strategy is **leader-follower**, where:

* A **single leader** handles all write operations.
* **Followers** replicate data and serve read operations.

The leader serializes writes to maintain consistency, while followers replicate the changes either synchronously or asynchronously.

### Synchronous vs. Asynchronous Replication

* **Synchronous Replication**:

  * Write is acknowledged only after all replicas persist the data.
  * Pros: Zero data loss.
  * Cons: High write latency and reduced availability—if any replica is down, the write fails.

* **Asynchronous Replication**:

  * Leader acknowledges the write immediately.
  * Followers replicate changes eventually.
  * Pros: Low write latency.
  * Cons: Risk of data loss if the leader crashes before replication completes.

* **Semi-Synchronous Replication**:

  * A hybrid setup where at least one follower is synchronous, others remain asynchronous.
  * Provides a balance between consistency and latency.

---

## Adding New Followers

When adding a new follower, it must perform:

1. **Snapshot Bootstrapping**: The leader sends a snapshot of the current state.
2. **Log Replay**: The follower replays changes from the leader’s write-ahead log (WAL, binlog, or translog).

If the replication logs are truncated before the follower catches up, the follower must restart from a fresh snapshot.

* PostgreSQL: Uses **Log Sequence Numbers (LSN)**.
* MySQL: Uses **binlog coordinates**.

---

## Failure Handling

* **Follower Failure**:

  * If short-lived, the follower resumes from the last checkpoint.
  * If prolonged, it may require a full re-index from the leader.

* **Leader Failure**:

  * A follower must be promoted to a new leader.
  * Clients must re-route writes to the new leader.
  * Other followers must re-align their replication targets.

---

## Replication Mechanisms

Replication relies on logs that record changes:

* **Elasticsearch**: Uses the **translog**.
* **PostgreSQL/MySQL**: Use WAL/binlogs.

### Statement-Based Replication

* Sends each SQL statement (INSERT, UPDATE, DELETE) to replicas.
* Pros: Simple to implement.
* Cons: Non-deterministic operations may lead to inconsistency.

### Logical (Row-Based) Replication / CDC

* Sends actual row-level changes.
* Compatible across versions.
* Common in **Change Data Capture (CDC)** systems.
* Used by tools like Debezium, Maxwell, and AWS DMS.

### Write-Ahead Log (WAL) Shipping

* A binary copy of the WAL is shipped periodically to replicas.
* Pros: Efficient for large-scale synchronization.
* Used in PostgreSQL’s physical replication mode.
* Trade-off: Not version tolerant; replicas must match leader version.

---

## Problems with Replication Lag

Replication lag occurs when followers fall behind the leader. This can lead to stale reads, especially problematic for applications expecting immediate consistency.

### Impacts of Lag:

* Violation of "Read-Your-Writes" consistency.
* Inconsistent analytics or user behavior tracking.

### Solutions:

* Monitor lag using log offsets, LSN, or heartbeat timestamps.
* Route critical reads to the leader.
* Apply **read fencing**—stall reads until replicas catch up.
* Use quorum reads to ensure sufficient agreement.

---

## Consistency Guarantees

### Read-Your-Writes

A client always sees its own updates. Achieved by routing reads to the leader or a caught-up follower.

### Monotonic Reads

A client never sees older data than what it previously saw. Enforced via version tracking or session stickiness.

### Strong Consistency Guarantees

To ensure **strong consistency**, systems often combine the following strategies:

* **Single-writer leadership** with **synchronous replication**.
* **Consensus protocols** like Raft or Paxos to elect and synchronize state.
* **Durable WAL writes** before acknowledgments.
* **Read-your-write routing policies**.
* **Quorum-based reads/writes**.

---

## Quorum-Based Consistency

A quorum system ensures consistency by requiring agreement from a majority of nodes.

In a system with **N** replicas:

* **W** = minimum number of nodes that must acknowledge a write
* **R** = minimum number of nodes that must respond to a read

To guarantee consistency:
**W + R > N**

This ensures that at least one node in every read quorum overlaps with a write quorum.

### Example:

* N = 3
* W = 2
* R = 2 → W + R = 4 > 3 → Consistency guaranteed

Tuning W and R lets you shift the balance between:

* **High Availability** (lower W/R)
* **Strong Consistency** (higher W/R)

---

## Advanced Replication Models

### Multi-Leader Replication (Multi-Master)

Multi-leader replication allows **multiple nodes to accept writes simultaneously**, often across geographically distributed data centers. This model improves **write availability**, **reduces latency**, and enables **regional independence**.

This approach is conceptually similar to collaborative applications like **Google Docs**, where multiple users can write concurrently, and the system merges their changes in real time. However, unlike Google Docs' operational transformation (OT) or CRDT-based reconciliation, database systems often rely on **conflict resolution strategies** that are less forgiving.

#### Benefits:

* Higher write throughput.
* Resilience to regional outages.
* Better user experience for globally distributed clients.

#### Challenges:

* **Conflict Resolution**: If two leaders modify the same record concurrently, conflicts must be resolved using strategies like:

  * **Last-write-wins (LWW)**
  * **Custom merge logic**
  * **Conflict-free Replicated Data Types (CRDTs)**

* **Eventual Consistency**: Data converges only after reconciliation.

* **Increased Complexity**: More coordination, schema management, and monitoring required.

#### Common Use Cases:

* Geo-distributed applications with low latency requirements.
* Offline-first applications with sync on reconnect.

#### Topologies:

* **Star Topology**: One central node replicates to others.
* **Ring Topology**: Nodes replicate to neighbors in a circular fashion.
* **Full Mesh**: Every node replicates to every other node.

Choosing a topology depends on network stability, consistency requirements, and write conflict probability.

---

## Handling Concurrent Writes and Version Vectors

In distributed systems—especially in multi-leader or leaderless setups—**concurrent writes** to the same key or document can lead to conflicts.

### Conflict Scenarios:

* Two users modify the same field simultaneously on different replicas.
* Network partitions allow writes on both sides that must later be merged.

### Conflict Detection and Resolution:

#### 1. **Version Vectors (Vector Clocks)**

* Each replica keeps a vector of counters for each other replica.
* When two versions are compared:

  * If one is strictly greater than the other, it is the newer version.
  * If neither is greater (concurrent updates), a conflict exists.

#### 2. **Last-Write-Wins (LWW)**

* Choose the version with the most recent timestamp.
* Simple, but can cause unintentional data loss.

#### 3. **Custom Merge Logic**

* Application-defined logic to resolve concurrent edits (e.g., merging lists).

#### 4. **Conflict-Free Replicated Data Types (CRDTs)**

* Data structures that automatically merge without conflict.
* Ideal for collaborative editing (e.g., counters, sets, text sequences).

Version vectors are especially helpful in identifying whether updates are causally related or concurrent, enabling systems to intelligently merge or flag conflicts.

---

## Leaderless Replication & Quorum-Based Consistency

In **leaderless replication** (e.g., Amazon Dynamo-style systems):

* Any node can accept reads or writes.
* Uses **quorum-based consistency** to ensure correctness.
* Supports **eventual consistency** with tuneable parameters for R/W.

Trade-offs:

* Pros: High availability, fault tolerance, simple horizontal scaling.
* Cons: Read/write conflicts, need for reconciliation, possible stale reads.
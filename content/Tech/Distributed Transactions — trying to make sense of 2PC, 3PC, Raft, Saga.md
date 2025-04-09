---
title: '"Transations - 2 PC and 3 PC "'
draft: 
tags:
  - database
  - transactions
  - Algorithm
  - consensus
  - Raft
  - Saga
  - distributed-transactions
---


Dumping my thoughts while trying to wrap my head around how distributed transactions are handled. Every system seems to solve this differently, and there’s no silver bullet.

---

## Two-Phase Commit (2PC)

Classic stuff. Coordinator talks to participants in 2 steps:

1. **Prepare**: ask everyone if they can commit
    
2. **Commit or Abort**: based on everyone's response
    

If all say yes, we go ahead. But if the coordinator crashes after prepare? Everyone’s stuck waiting. Can’t decide on their own. Basically: **blocking**.

### Why use it?

If you're working in environments where you absolutely need ACID properties and strong coordination (e.g. financial systems, banking middleware), and you're okay with availability tradeoffs.

### Where is it used?

- XA transactions in JDBC, JTA (Java Transaction API)
    
- Oracle, IBM DB2, SQL Server — mostly in enterprise environments
    
- Sometimes used in Kafka for exactly-once semantics (transactional producer)
    

### Pain points

- Not fault-tolerant under coordinator failure
    
- Doesn’t scale well with partitions
    
- Recovery is a headache if things crash mid-protocol
    

---

## Three-Phase Commit (3PC)

Tried to fix 2PC’s blocking problem by splitting it into 3 steps:

1. Can commit?
    
2. Pre-commit
    
3. Commit
    

The idea: participants can time out and move forward instead of waiting forever. But still not totally safe under network partitions. Not really used in the wild — too much complexity, not enough payoff.

### Why it's interesting (in theory)

- Tries to be non-blocking by giving nodes a way to make progress on timeout
    

### Why it's _not_ used

- Still vulnerable in real distributed settings with network partitions
    
- Hard to implement and test
    

Mostly academic. No real adoption in modern infra.

---

## Raft (or Paxos)

Shift in mindset. Not about transactions per se, but **consensus**. Everyone agrees on the same log of operations.

- One leader
    
- Followers replicate logs
    
- Majority-based decisions
    
- Leader failure? Re-elect
    

This is more about replicating state machines and maintaining a consistent cluster.

### Why use it?

- You want fault-tolerant strong consistency
    
- Good for replicated databases or distributed system coordination
    

### Where is it used?

- etcd (Kubernetes coordination)
    
- Consul
    
- CockroachDB
    
- TiKV (underpinning TiDB)
    
- RQLite
    

### Tradeoffs

- Slower than 2PC in happy path due to log replication + quorum writes
    
- Better guarantees under partitions/failure though
    

Good if you’re building infra that needs consistent metadata/state across nodes.

---

## Saga Pattern

Totally different take. Embrace the fact that distributed transactions suck and just... don’t do them.

Instead:

- Break the transaction into steps
    
- Each step is a local tx
    
- If one fails, run a compensating transaction (e.g., cancel order, refund payment)
    

### Why use it?

- Works well in microservice-based architectures
    
- Prioritizes availability over strict atomicity
    
- Doesn’t block — everything is async
    

### Where it's used?

- E-commerce order flows
    
- Booking engines (flights, hotels)
    
- Financial services (with eventual consistency)
    
- Systems using Temporal, Camunda, Apache Airflow
    

### Gotchas

- Compensating actions can be hard/impossible to write (e.g. what’s the undo for sending an email?)
    
- No global atomicity guarantee
    

---

## Rough Comparison

||2PC|3PC|Raft|Saga|
|---|---|---|---|---|
|Atomic?|Yes|Yes|Yes (via consensus)|Not strictly|
|Blocking?|Yes|Less|No|No|
|Partition-tolerant?|No|No|Yes|Yes|
|Use cases|DB transactions|Academic|Distributed DBs|Microservices|
|Complexity|Medium|High|High|Medium|

---

### 2PC in the wild

- **PostgreSQL**: Has explicit `PREPARE TRANSACTION` / `COMMIT PREPARED` support. You can actually get stuck in “prepared” limbo if something crashes midway.
- **MySQL (InnoDB)**: Supports XA transactions; tricky to use right, and often disabled.
- **Oracle, SQL Server, IBM DB2**: Built-in 2PC. This is where it was born — tightly coupled infra with shared storage or very reliable networks.
- **Kafka**: Not exactly 2PC, but its producer+broker interaction when `enable.idempotence=true` + `transactional.id` is kind of a 2PC pattern.
- **Spring Boot / JTA / Atomikos**: Java EE's take on 2PC using transaction managers that coordinate DB + JMS.

>  Reality: 2PC shows up in monoliths or tightly coupled enterprise systems — not microservices or anything cloud-native.

---

### Raft-based distributed DBs

- **CockroachDB**: Every range of data is managed by a Raft group. Transactions use MVCC + Raft consensus to get serializability without classic 2PC.
- **TiDB / TiKV**: Inspired by Google Spanner. TiKV is the key-value layer that uses Raft; TiDB is the MySQL-compatible SQL layer.
- **YugabyteDB**: Combines PostgreSQL front-end with Raft-based storage layer.
- **etcd**: Critical for Kubernetes — uses Raft to maintain config/state consistency across control plane nodes.
- **Dgraph**: Graph DB that uses Raft to coordinate cluster membership and replication.

>  These systems **avoid 2PC entirely** by embedding consensus into their replication + commit layers.

---

### Saga-based Systems

- **Temporal**: You write workflows in code, it takes care of retries, compensation, state transitions.
- **Netflix Conductor**: Workflow engine used internally at Netflix to coordinate Sagas across services.
- **Camunda**: BPMN + Java DSLs for modeling distributed processes with compensation.
- **Apache Airflow** (loosely): not transactional, but can orchestrate compensating steps.
- **Stripe / Revolut / Uber**: Tend to use event sourcing + Sagas, avoiding any need for cross-service transactions.

>  Writing good **compensation logic is hard**. What’s the undo of a push notification? A refund? A message retraction?

---

##  Common Pitfalls / Anti-patterns

- **“Let’s just use 2PC between microservices”** — This is how people end up in hell. You can’t guarantee reachability or agreement across independently deployed services.
- **“We'll retry until it works”** — Good luck without idempotency. You'll double-charge users, double-book flights.
- **"We’ll write compensating logic later"** — You won’t. And if you do, it’ll be flaky and incomplete.
- **Mixing orchestration + choreography** — Easy to get lost in who calls who and when. Leads to brittle inter-service contracts.

---

##  How to think about picking one

| Question                                                | Think about                                                                 |
| ------------------------------------------------------- | --------------------------------------------------------------------------- |
| Do I control the full stack?                            | 2PC might be okay.                                                          |
| Is availability more important than strict consistency? | Go Saga.                                                                    |
| Am I building infrastructure or coordination layer?     | Use Raft.                                                                   |
| Do I need external DB + message broker coordination?    | Be careful. Very few systems get this right (e.g. Kafka’s transaction API). |
| Do I want retries, compensation, timeouts natively?     | Saga workflow engines like Temporal.                                        |

----

##  Related Systems I Want to Dig Into

- [ ] Google **Spanner**: Uses TrueTime + Paxos for global serializability. No 2PC. Clocks and replicas, not coordination.
- [ ] **Percolator** (Google): used in BigTable for multi-row transactions. Precursor to Spanner?
- [ ] **DynamoDB** vs **Raft-style** systems — how does eventual consistency trade off with quorum-based approaches?
- [ ] **RAMP Transactions**: Read Atomic Multi Partition — avoids coordination on reads.
- [ ] **TLA+ specs** for 2PC and Raft — worth modeling to build intuition?
- [ ] Research how **Flink**, **Spark**, and **Kafka Streams** do checkpoints and exactly-once guarantees — do they secretly use 2PC?


---

## Takeaways

- If I need ACID across services, **2PC or Raft**.
    
- If I care more about uptime than perfect consistency, **Saga**.
    
- If I’m building infra (like a DB or scheduler), Raft is probably the better long-term bet.
    
- Don’t bother with 3PC unless I’m writing a paper.
    

Honestly, the real answer to "how to do distributed transactions" is usually: **don’t.** Just make your system eventually consistent, use idempotency, and write good compensation logic.


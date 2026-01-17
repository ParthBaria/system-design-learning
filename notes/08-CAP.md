# CAP Theorem

In a distributed computer system, you can only guarantee **two out of the following three properties** at the same time:

- **Consistency (C)**  
  Every read receives the most recent write or an error.

- **Availability (A)**  
  Every request receives a response, but it may not contain the most recent data.

- **Partition Tolerance (P)**  
  The system continues to operate despite network failures that split the system into partitions.

Since networks are unreliable, **Partition Tolerance is mandatory** in distributed systems.  
This forces a tradeoff between **Consistency** and **Availability**.

---

## CAP Combinations

### CP – Consistency + Partition Tolerance
- The system prioritizes consistent data.
- Requests may fail or timeout during partitions.
- Suitable when atomic reads and writes are required.

**Examples:** MongoDB, HBase

---

### AP – Availability + Partition Tolerance
- The system remains responsive even during partitions.
- Data may be stale or inconsistent temporarily.
- Writes propagate asynchronously and resolve later.

**Examples:** Cassandra, DynamoDB

AP is suitable when **eventual consistency is acceptable** and availability is critical.

---

### CA – Consistency + Availability
- Works only when there are **no network partitions**.
- Typical of traditional single-node or tightly coupled systems.

**Examples:** Traditional RDBMS (single-node setups)

---

## ATM Example (CAP Tradeoff)

ATMs are connected through distributed nodes.

- If the system prioritizes **Consistency**:
  - All ATM nodes must agree on the account balance.
  - During a network partition, withdrawals may fail.
  - User cannot read or write until consistency is restored.

- If the system prioritizes **Availability**:
  - ATMs continue operating during partitions.
  - Different nodes may show different balances.
  - This can lead to data inconsistency.

---

## Consistency Models

### Read-After-Write Consistency
A user can read updated data immediately after a write operation completes, even in a distributed system.

---

## Forms of Consistency

### Linearizability
- Operations appear instantaneous to clients.
- The system behaves like a single, non-replicated datastore.
- Also known as **strong consistency** or **read-after-write consistency**.

**Implementation:**  
Synchronous replication (e.g., single-master replication).

---

### Sequential Consistency
- All nodes see operations in the same order.
- Timing does not matter; ordering does.

Leader and followers must execute operations in the same sequence, even if followers apply them later.

---

### Causal Consistency
- Preserves cause-and-effect relationships.
- If Event A causes Event B, all nodes observe A before B.

**Example:**  
Social media comments and replies must appear in logical order.

---

### Eventual Consistency
- No strict ordering guarantees.
- Updates propagate asynchronously.
- All replicas eventually converge to the same state.

**Example:**  
Seeing a Facebook post a few seconds later is acceptable.

This is the weakest but most scalable consistency model.

---

# CAP in Big Companies

## Google

### Strong-Consistency Systems
- **Spanner (CP)**: Global relational database using Paxos + TrueTime.
- Provides external consistency across data centers.
- Used where strict correctness is required.

### Availability-Focused Systems
- Search indexes, caches, analytics systems.
- Accept stale reads and eventual consistency.

**Why:**  
Google supports both enterprise correctness and massive low-latency consumer services by investing heavily in distributed coordination.

---

## Facebook

### Availability-First Systems
- Feeds, likes, timelines, and social graph reads.
- Heavy use of caching (Memcached) and async replication.
- Eventual consistency is acceptable.

### Strong Consistency Where Needed
- User accounts
- Payments
- Critical write operations

**Why:**  
User experience and responsiveness matter more than immediate consistency for most interactions.

---

## Quora

### Read-Heavy Architecture
- Questions, answers, votes, and feeds favor availability.
- Caching and async replication are common.

### Stronger Consistency for:
- Edits
- Ownership changes
- Conflict resolution

**Why:**  
Fast browsing matters more than instant correctness of non-critical data.

---

## Practical CAP Tradeoffs

### Hybrid Architectures
- CP systems for metadata, ownership, and transactions
- AP systems for caching, feeds, and search

### Multi-Level Consistency
- Strong consistency for writes and critical reads
- Eventual consistency for bulk reads

### Techniques Used
- Read-repair
- Conflict resolution
- Causal consistency
- Version vectors

### Operational Strategies
- Graceful degradation
- Feature flags
- Client-side retries and fallbacks

### Business-Driven Decisions
- Financial and billing systems → Consistency
- Feeds, recommendations, discovery → Availability

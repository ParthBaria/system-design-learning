# Sharding

Sharding distributes data across different databases such that each database manages only a subset of the data.  
Taking a users database as an example, as the number of users increases, more shards are added to the cluster.

Similar to federation, sharding results in:
- Less read and write traffic per database
- Less replication overhead
- More cache hits
- Smaller index sizes, improving query performance

If one shard goes down, other shards remain operational. However, replication is usually added to avoid data loss.  
There is no single central master serializing writes, allowing parallel writes and increased throughput.

Common ways to shard a users table:
- By user's last name initial
- By geographic location

---

## Vertical vs Horizontal Partitioning

| Feature | Vertical Partitioning | Horizontal Partitioning |
|------|----------------------|------------------------|
| **Definition** | Dividing a table into smaller tables based on columns | Dividing a table into smaller tables based on rows |
| **Purpose** | Reduce column count to improve performance | Manage large volumes of rows efficiently |
| **Data distribution** | Related columns stored together | Related rows grouped by condition or range |
| **Query performance** | Faster when accessing specific columns | Faster when accessing subsets of rows |
| **Maintenance and indexing** | Easier column-based indexing | Independent indexing per partition |
| **Joins** | Often required across partitions | Usually not required |
| **Data integrity** | Harder to maintain across partitions | Easier to maintain |
| **Use cases** | Tables with many infrequently used columns | Tables with massive row counts |
| **Examples** | Splitting customer details and transaction history | Partitioning orders by date or region |

---

## Sharding Strategies

1. **Range-Based Sharding**  
   Data is partitioned based on a value range such as dates or IDs.  
   Example:  
   - IDs 1–10000 → Shard 1  
   - IDs 10001–20000 → Shard 2  

2. **Hash-Based Sharding**  
   A hash function maps data to shards using a key like user ID.  
   Advantage: Even data distribution and balanced load.

3. **Directory-Based Sharding**  
   A lookup table maps each data item to a shard.  
   Useful when data frequently moves between shards.

4. **Round-Robin Sharding**  
   Data is assigned to shards sequentially.  
   Simple to implement but offers limited control over distribution.

---

## Challenges of Sharding

- **Complexity & Management**  
  Increased operational overhead for monitoring, backups, and maintenance.

- **Data Consistency**  
  Distributed ACID transactions are difficult to maintain.

- **Cross-Shard Queries**  
  Slow and complex due to data aggregation across shards.

- **Uneven Distribution (Hotspots)**  
  Poor shard key selection overloads specific shards.

- **Rebalancing**  
  Moving data between shards is complex and time-consuming.

- **Sharding Key Selection**  
  A bad key leads to long-term scalability problems.

- **Global Transactions**  
  Atomic transactions across shards are difficult.

- **Elasticity**  
  Adding or removing shards dynamically is disruptive.

---

## Disadvantages of Sharding

- Application logic must be shard-aware
- Complex SQL queries
- Uneven load due to power users
- Difficult joins across shards
- Rebalancing complexity
- Additional hardware and operational cost

---

# Replication

Replication is commonly used in:
- Master-Slave
- Master-Master architectures in RDBMS

---

## Master-Slave Replication

- Master handles reads and writes
- Slaves replicate writes and serve reads
- Slaves can replicate further in a tree structure
- If master fails, system continues in read-only mode until failover

---

## Master-Master Replication

- Both masters handle reads and writes
- Write coordination is required
- If one master fails, the other continues full operation

---

## Importance of Replication in Distributed Systems

- **High Availability**: Access data even during node failures
- **Reliability**: Eliminates single points of failure
- **Low Latency**: Replicas closer to users reduce response time
- **Scalability**: Handle more users by adding replicas

---

## Benefits of Replication

- Enhanced availability
- Improved performance
- Better scalability
- Fault tolerance
- Load balancing

---

## Disadvantages of Replication

- Potential data loss if master fails before replication
- Write-heavy systems cause replication lag
- More replicas increase synchronization overhead
- Read replicas may write sequentially
- Additional hardware and complexity

---

## When to Use Sharding

1. A single database becomes overloaded with large request volume
2. Apply horizontal partitioning to split users across databases
3. Add replication to handle increased read load
4. Partition by region if users mostly access local data
5. Use sharding when scale exceeds partitioning + replication limits

⚠️ Note:
- ACID transactions are hard to maintain
- Each write requires connecting to the correct shard
- Connection pooling becomes critical

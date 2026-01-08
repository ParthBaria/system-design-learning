# sharding

Sharding distributes data across different databases such that each database can only manage a subset of the data. Taking a users database as an example, as the number of users increases, more shards are added to the cluster.

Similar to the advantages of federation, sharding results in less read and write traffic, less replication, and more cache hits. Index size is also reduced, which generally improves performance with faster queries. If one shard goes down, the other shards are still operational, although you'll want to add some form of replication to avoid data loss. Like federation, there is no single central master serializing writes, allowing you to write in parallel with increased throughput.

Common ways to shard a table of users is either through the user's last name initial or the user's geographic location.

| Feature                      | Vertical Partitioning                                                                               | Horizontal Partitioning                                                                                   |
| ---------------------------- | --------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **Definition**               | Dividing a table into smaller tables based on columns.                                              | Dividing a table into smaller tables based on rows (usually ranges of rows).                              |
| **Purpose**                  | Reduce the number of columns in a table to improve query performance and reduce I/O.                | Divide a table into smaller tables to manage large volumes of data efficiently.                           |
| **Data distribution**        | Columns with related data are placed together in the same table.                                    | Rows with related data (typically based on a range or a condition) are placed together in the same table. |
| **Query performance**        | Improves query performance when queries only involve specific columns that are part of a partition. | Improves query performance when queries primarily access a subset of rows in a large table.               |
| **Maintenance and indexing** | Easier to manage and index specific columns based on their characteristics and access patterns.     | Each partition can be indexed independently, making indexing more efficient.                              |
| **Joins**                    | May require joins to combine data from multiple partitions when querying.                           | Joins between partitions are typically not needed, as they contain disjoint sets of data.                 |
| **Data integrity**           | Ensuring data consistency across partitions can be more challenging.                                | Easier to maintain data integrity, as each partition contains a self-contained subset of data.            |
| **Use cases**                | Used for tables with many columns where not all columns are frequently accessed together.           | Used for tables with a large number of rows that can be grouped by criteria like date or region.          |
| **Examples**                 | Splitting a customer table into personal details and transaction history tables.                    | Partitioning a sales order table by date, such as monthly or yearly partitions.                           |

# sharding stretigies

1.Range-based sharding: In this strategy, data is partitioned based on a predefined range of values, such as a range of dates or IDs. For example, all customer data with IDs from 1 to 10000 may be stored on one shard, while customer data with IDs from 10001 to 20000 may be stored on another shard.
2.Hash-based sharding: In this strategy, data is partitioned based on a hash function that maps data to a specific shard. The hash function can be based on any attribute of the data, such as customer ID or order ID. The advantage of hash-based sharding is that it evenly distributes the data across all shards, which helps to balance the workload.
3.Directory-based sharding: In this strategy, a directory or lookup table is used to map data to a specific shard. The directory contains information about which shard a particular piece of data is stored on. This strategy can be useful in situations where the data is frequently updated or moved between shards.
4.Round-robin sharding: In this strategy, data is distributed across shards in a round-robin fashion. Each piece of data is assigned to the next available shard in a sequence. This strategy is simple to implement and works well when the data is evenly distributed.





# challenges

Complexity & Management: More components mean more overhead for monitoring, backups, and maintenance, increasing operational costs.
Data Consistency: Ensuring all shards have up-to-date data, especially with distributed transactions (ACID compliance), is difficult.
Cross-Shard Queries: Queries needing data from multiple shards are slow and complex, requiring data aggregation.
Uneven Distribution (Hotspots): Poor key selection leads to some shards being overloaded (hotspots) while others are idle, defeating scalability goals.
Rebalancing: Moving data to balance loads is a complex, time-consuming process, potentially causing downtime.
Sharding Key Selection: Choosing the right key is crucial; a bad choice can lead to inflexibility and imbalance.
Global Transactions: Maintaining atomicity for transactions spanning multiple shards is a major hurdle.
Elasticity: Dynamically adding or removing shards can be challenging and disruptive.

## Disadvantage(s): sharding
You'll need to update your application logic to work with shards, which could result in complex SQL queries.
Data distribution can become lopsided in a shard. For example, a set of power users on a shard could result in increased load to that shard compared to others.
Rebalancing adds additional complexity. A sharding function based on consistent hashing can reduce the amount of transferred data.
Joining data from multiple shards is more complex.
Sharding adds more hardware and additional complexity.

# replication

it used in master slave and master-master techniques of RDBMS.

Master-slave replication
The master serves reads and writes, replicating writes to one or more slaves, which serve only reads. Slaves can also replicate to additional slaves in a tree-like fashion. If the master goes offline, the system can continue to operate in read-only mode until a slave is promoted to a master or a new master is provisioned.

Master-master replication
Both masters serve reads and writes and coordinate with each other on writes. If either master goes down, the system can continue to operate with both reads and writes.

## Importance of Replication in Distributed Systems
Replication plays a crucial role in distributed systems due to several important reasons:
Enhanced Availability: Replication ensures the system stays available even if some nodes fail, as users can access data from other healthy replicas.
Improved Reliability: With multiple copies of data, the system avoids single points of failure, ensuring continuous operation.
Reduced Latency: Replicas placed closer to users reduce access time, improving speed and user experience.
Scalability: Replication spreads the workload across nodes, allowing the system to handle more users or data by adding more replicas as needed.

## Benefits of Replication in Distributed Systems
Below are the benefits of replication in distributed systems:

Enhanced Availability: Replication keeps data accessible even if some nodes fail, reducing downtime.
Improved Performance: Placing replicas closer to users lowers latency and boosts response time.
Scalability: Replicas distribute the load, allowing the system to scale with user demand.
Fault Tolerance: If one replica fails, others take over, ensuring uninterrupted service.
Load Balancing: Replication spreads requests across nodes, preventing overload and improving efficiency.

## Disadvantage(s): replication

There is a potential for loss of data if the master fails before any newly written data can be replicated to other nodes.
Writes are replayed to the read replicas. If there are a lot of writes, the read replicas can get bogged down with replaying writes and can't do as many reads.
The more read slaves, the more you have to replicate, which leads to greater replication lag.
On some systems, writing to the master can spawn multiple threads to write in parallel, whereas read replicas only support writing sequentially with a single thread.
Replication adds more hardware and additional complexity.



## when to use sharding

if one have database and it have large requests then it overload and not able to handle it

first we can do horizontal partitioning in which we divide user in mutiple databases

then we can use reliacation if user loads increase. like master backup techniques whre master writes and backup for reads.

then if it does not work then we use divide them by region which mostly time one region user does not always have work on diffrent region databse

then it it goes beyond control then we use sharding.you can not maintain acid transactions.
and for every write request you need to establish connection to server.for that particualr user.
/pooling concept


# CAP thearom

In a distributed computer system, you can only support two of the following guarantees:

Consistency - Every read receives the most recent write or an error
Availability - Every request receives a response, without guarantee that it contains the most recent version of the information
Partition Tolerance - The system continues to operate despite arbitrary partitioning due to network failures
Networks aren't reliable, so you'll need to support partition tolerance. You'll need to make a software tradeoff between consistency and availability.

CP - consistency and partition tolerance
Waiting for a response from the partitioned node might result in a timeout error. CP is a good choice if your business needs require atomic reads and writes.
mongodb and hbase

AP - availability and partition tolerance
Responses return the most readily available version of the data available on any node, which might not be the latest. Writes might take some time to propagate when the partition is resolved.
casandra and dynamodb

AP is a good choice if the business needs to allow for eventual consistency or when the system needs to continue working despite external errors.

CA- traditional rdbms 


## example atm

atms are connected through nodes

if it chooses conssitency it will maintain consistent data across nodes.
but if partition happen then use will not able to read and write

if it chooes availability then data become un cossistent if partition happen one atm node will update data but not other it can have data conssistency error


## consistnccy models

read after write consistency 

user will only able to read after write happens by other node in partition system.

### forms of consistency

Linearizability

A system that supports linearizable consistency is one where operations appear instantaneous to the external client. It essentially behaves as if the system isn’t replicated but operates on a single dataset, but in reality, it’s distributed and replicated.

This is the type of consistency we’ve seen thus far, also called Read after Write Consistency.

Synchronous Replication, which we saw in Single Master Replication, ensures that the system follows Linearizability.

This form of consistency ensures that operations from different clients need to be viewed in the same order by all clients.

Sequential Consistency

This essentially means that the Leader and the followers must execute the operations in the same order.

This is a case of Sequential Consistency since the order of writes is maintained across the leader and followers. It doesn’t matter if followers perform their writes much later than the leader; they just have to ensure that the order of operations is the same.

Causal Consistency

When Event A directly influences the occurrence of Event B, we say that Event A caused Event B. This concept was explained in detail in Message Ordering.

Causal Consistency ensures that causally related events have the same operational order in all nodes of the system. For example, in a social media platform, it is important to show comments and their replies in the correct order. If the order is reversed, the conversation would not make sense. Here, the comment and its replies are causally related and must be maintained in order in all nodes.

Eventual Consistency

This is the weakest form of consistency where no ordering is maintained. It simply ensures that all operations are eventually propagated to all nodes in any order possible. This is useful for systems that do not need very strong consistency assumptions.

For example, when your friend creates a new post on Facebook, it is not crucial that you see it immediately. It is acceptable to see it 10 seconds later or even a bit later than that. Hence, the replication for post creation can use Eventual Consistency.

# for big companies

Google

Strong-consistency choices:
Spanner (CP-style): globally-distributed relational-like storage that provides external consistency ( ) using Paxos + TrueTime. Intended for services that need strong correctness across datacenters.
Bigtable and Colossus can be configured for stronger consistency where required.
Availability-focused choices:
Many caching layers, search index replicas, and analytics are designed for high availability and low latency; they accept eventual consistency or stale reads.
Why: Google builds infrastructure for enterprise workloads needing strict correctness (cloud databases, advertising auctions) and also for massive-scale, low-latency user services. They invest in distributed coordination (TrueTime) to expand the CP region without sacrificing global semantics.

Facebook

Availability-first patterns:
Social graph reads, feeds, likes, and timelines prioritize low latency and high availability. Systems use extensive caching (Memcached tiers), asynchronous replication, and eventually-consistent stores.
TAO (Facebook’s graph data store) gives fast reads with relaxed consistency guarantees for many read paths and uses synchronous operations selectively.
Stronger consistency where needed:
User account operations, monetary flows, and certain write-critical flows use synchronous replication or transactional semantics.
Why: User experience and responsiveness are paramount; occasional visible inconsistencies (e.g., slightly delayed likes) are acceptable compared to a slower or unavailable service.

Quora

Read-heavy content platform:
Answers, questions, votes and feeds are optimized for availability and latency; caches and asynchronous replication are widely used.
Edit conflicts and ownership changes require stronger consistency or conflict-resolution logic.
Why: The typical Quora user expects fast browsing; slight delays in propagation (e.g., vote counts) are tolerable, while correctness of edits and authorship must be preserved.
How these tradeoffs are implemented in practice

Hybrid architectures:
Use CP systems for metadata, transactions, and ownership; use AP systems for caching, feeds, and search indices.
Employ multi-level consistency: strong on writes/critical reads, eventual for bulk reads.
Use read-repair, conflict resolution, causal consistency, and version vectors to reduce user-visible anomalies while preserving availability.
Operational controls:
Graceful degradation (feature flags, degraded read modes) to maintain availability during partitions.
Client-side fallbacks and retries to mask temporary inconsistencies.
Business-driven choices:
Critical financial/advertising systems (billing, bidding) trend toward consistency.
Engagement and discovery systems (feeds, recommendations) prioritize availability and latency.
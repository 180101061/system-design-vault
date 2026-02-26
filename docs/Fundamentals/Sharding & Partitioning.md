![Sharding and Partitioning](../Diagrams/Sharding_and_Partitioning.png)
## Core Definitions

### Partitioning
Dividing a dataset into smaller logical pieces.
- **Vertical partitioning** → Split by columns
- **Horizontal partitioning** → Split by rows
> Sharding = horizontal partitioning across multiple machines.
---
## Why Sharding?
### Problems Without Sharding
- Vertical scaling limit (CPU, RAM, Disk)
- Single point of failure
- Write throughput bottleneck
- Large index degradation
### Goals
- Horizontal scalability
- Higher write throughput
- Fault isolation
- Reduced index size per node
---
## Types of Partitioning
### A) Vertical Partitioning
Split by columns.
Example:  
User table:
`(id, name, email) → Table A (profile_pic, bio) → Table B`
Used for:
- Reducing I/O
- Separating hot vs cold data
---
### B) Horizontal Partitioning (Sharding)
Split rows across shards:
`Shard 1 → user_id 1–1M Shard 2 → user_id 1M–2M`

---
## Sharding Strategies (Very Important)
---
### 4.1 Range-Based Sharding
Partition by value range.
Example:
`0–1000 → Shard 1 1001–2000 → Shard 2`
#### Pros
- Efficient range queries
- Simple routing
#### Cons
- Hotspot risk
- Uneven distribution
- Difficult resharding
---
### 4.2 Hash-Based Sharding
`shard_id = hash(key) % N`
#### Pros
- Uniform distribution
- Avoids skew (generally)
#### Cons
- Poor for range queries
- Adding shard → full rehash
---
### 4.3 Consistent Hashing (High-Value Topic)
Used in:
- Apache Cassandra
- Amazon DynamoDB
- Redis Cluster
#### Core Idea
- Map nodes + keys on hash ring
- Key assigned to next clockwise node
#### Key Advantage
When adding/removing node:
- Only small portion of keys move
- No global rehash
#### Interview Line

> Consistent hashing minimizes key redistribution during cluster scaling.
---
## Shard Key Selection (Extremely Important)
Bad shard key = production disaster.
### Good Shard Key Properties
- High cardinality
- Uniform distribution
- Frequently used in queries
- Immutable
- Avoid monotonic growth
### Bad Examples
- `country` → low cardinality
- `created_at` → hotspot on latest shard
- Boolean fields
### Good Examples
- `user_id`
- `order_id`
- UUID
---
## Hotspot Problem
Even with hashing:
- Celebrity accounts
- Trending content
- Write-heavy users
### Solutions
- Key salting
- Logical partitioning
- Caching (e.g., Redis)
- Write spreading
- Rate limiting
---
## Rebalancing
Triggered when:
- Adding/removing shards
- Skew detection
- Storage threshold exceeded
### Problems
- Data migration cost
- Network congestion
- Replica lag
- Temporary performance drop
Modern DBs rebalance gradually.
---
## Sharding + Replication
Sharding ≠ Replication

| Concept     | Purpose      |
| ----------- | ------------ |
| Sharding    | Scale        |
| Replication | Availability |

Example:  
Each shard has:
- 1 Primary
- 2 Replicas
Used in:
- MongoDB
- MySQL
- PostgreSQL
---
## Cross-Shard Queries
Single-shard query:
`SELECT * FROM users WHERE user_id = X`
Cross-shard query:
`SELECT * FROM orders WHERE amount > 1000`
Requires:
- Scatter-gather
- Merge results
- Global sort
### Problems
- High latency
- Expensive
- Hard to maintain strong consistency
### Solutions
- Denormalization
- Secondary indexes
- Data duplication
- Analytics DB separate
---
## Distributed Transactions
Across shards:
- Require 2PC (Two-Phase Commit)
- Slow
- Complex
- Reduce availability

Systems:
- Google Spanner → Strong consistency
- Apache Cassandra → Tunable consistency

Interview Tip:  
Avoid cross-shard transactions in design unless necessary.

---
## CAP Theorem Context
In distributed sharded systems:
Must tolerate network partitions.
Tradeoff between:
- Consistency
- Availability
Most NoSQL systems favor:

> Availability + Partition tolerance
---
## Directory-Based Sharding
Instead of computing shard:
Maintain lookup table:
`user_id → shard_id`
### Pros
- Flexible
- Easy resharding
### Cons
- Directory becomes bottleneck
- Extra lookup cost
---
## Geo-Sharding
Partition by region:
`India users → India cluster US users → US cluster`
Used for:
- Data locality
- Compliance (GDPR)
- Low latency
---
## Common Interview Questions
### Q1: Why not vertical scaling?
Hardware limit + cost + SPOF.
### Q2: What happens when a shard fills?
Resharding or data migration.
### Q3: Can we change shard key?
Yes, but extremely expensive migration.
### Q4: Why avoid cross-shard joins?
Require coordination → latency + complexity.
### Q5: How do you handle uneven traffic?
Rebalancing + virtual nodes + caching.

---
## Quick Comparison

|Strategy|Uniform|Range Queries|Easy Scaling|
|---|---|---|---|
|Range|❌|✅|❌|
|Hash|✅|❌|❌|
|Consistent Hash|✅|❌|✅|

---
## Practical Design Heuristics
- Shard by access pattern, not by intuition.
- Keep related data in same shard.
- Avoid cross-shard transactions.
- Replication factor ≥ 3.
- Monitor shard size & QPS.
- Plan resharding from day 1.
  
  
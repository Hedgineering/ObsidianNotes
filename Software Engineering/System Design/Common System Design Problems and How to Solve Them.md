# Scaling Problems

## Hot Key Problem

### Symptoms

One key receives massive traffic.

Examples:

- Trending Twitter post
- Viral YouTube video
- Popular stock ticker

### Solutions

- Replicate hot data
- Local caches
- CDN
- Shard key artificially

Example:

user:123

becomes

user:123:0
user:123:1
user:123:2

---

## Cache Stampede (Thundering Herd)

### Symptoms

Cache entry expires.

Thousands of requests simultaneously hit DB.

### Solutions

- Request coalescing
- Single-flight locking
- Cache warming
- Staggered TTLs
- Soft expiration

Common interview answer:

```text
Redis lock
+
single request rebuilds cache
```

---

## Cache Avalanche

### Symptoms

Many cache entries expire simultaneously.

Entire DB gets overwhelmed.

### Solutions

- Randomized TTL
- Cache warming
- Multiple cache layers

---

## Cache Penetration

### Symptoms

Repeated requests for non-existent data.

Always misses cache.

Always hits DB.

### Solutions

- Bloom filter
- Cache null responses
- Input validation

---

# Distributed Systems Problems

## Split Brain

### Symptoms

Cluster partitions.

Multiple leaders elected.

Conflicting writes occur.

### Solutions

- Consensus algorithms
- Quorum voting
- ZooKeeper
- Raft
- Paxos

---

## Leader Failure

### Symptoms

Primary database dies.

### Solutions

- Leader election
- Automatic failover
- Consensus protocol

Examples:

- Raft
- Paxos

---

## Network Partition

### Symptoms

Nodes lose communication.

### CAP Decision

Must choose:

- Consistency
- Availability

Cannot guarantee both.

---

# CAP Theorem

## C = Consistency

Every read sees latest write.

---

## A = Availability

Every request gets response.

---

## P = Partition Tolerance

System survives network failures.

---

## CP Systems

Choose:

```text
Consistency + Partition Tolerance
```

Lose availability.

Examples:

- ZooKeeper
- etcd
- Spanner (mostly CP)

Use when:

- Financial transactions
- Metadata
- Distributed locks

---

## AP Systems

Choose:

```text
Availability + Partition Tolerance
```

Lose consistency.

Examples:

- Cassandra
- DynamoDB
- DNS

Use when:

- Social media
- Metrics
- Recommendation systems

---

# PACELC

CAP only describes failures.

PACELC adds normal operation tradeoffs.

---

## PACELC

If Partition:

```text
Availability vs Consistency
```

Else:

```text
Latency vs Consistency
```

---

## Examples

### DynamoDB

```text
PA/EL
```

Prioritizes:

- Availability
- Low latency

---

### Spanner

```text
PC/EC
```

Prioritizes:

- Consistency

Accepts extra latency.

---

# Consistent Hashing

## Problem

Adding server changes all hashes.

Traditional:

```text
hash(key) % N
```

Adding server:

```text
N changes
```

Everything moves.

---

## Solution

Hash ring.

Only small portion of keys move.

---

## Used In

- Cassandra
- DynamoDB
- Memcached
- Distributed caches

---

## Virtual Nodes

Each machine owns many positions.

Benefits:

- Better load balancing
- Easier scaling

---

# Rate Limiting

## When To Use

Protect:

- APIs
- Authentication
- Databases
- Expensive endpoints

---

## Token Bucket

Most common.

Tokens refill continuously.

Allows bursts.

Used by:

- AWS APIs
- Most gateways

---

## Leaky Bucket

Smooth output rate.

Less bursty.

---

## Sliding Window

Most accurate.

More expensive.

---

## Fixed Window

Simple.

Can have boundary issues.

---

# Consensus

## Goal

Many machines agree on state.

---

## Raft

Most interview-friendly.

Concepts:

- Leader
- Follower
- Election
- Log replication

---

## Paxos

More complex.

Rarely expected in detail.

---

# Quorums

## Formula

```text
R + W > N
```

where:

R = read quorum
W = write quorum
N = replicas

Guarantees overlap.

---

## Example

N = 3

Choose:

```text
R=2
W=2
```

Any read intersects latest write.

---

# Database Scaling

## Read Replica

### Use When

```text
Reads >> Writes
```

Benefits:

- Offload reads

Problems:

- Replication lag

---

## Sharding

### Use When

Single DB cannot scale.

---

### Common Keys

- user_id
- account_id
- customer_id

---

### Bad Shard Keys

Low cardinality.

Example:

```text
country
```

creates imbalance.

---

# Messaging Problems

## Queue Backlog

### Symptoms

Consumers slower than producers.

### Solutions

- More consumers
- Batch processing
- Backpressure

---

## Poison Message

### Symptoms

One message always fails.

### Solutions

- Retry count
- Dead Letter Queue (DLQ)

---

# Reliability Patterns

## Retry

Use for:

- Transient failures

Avoid for:

- Validation errors
- Bad requests

---

## Exponential Backoff

Example:

```text
1s
2s
4s
8s
```

Prevents retry storms.

---

## Circuit Breaker

### Use When

Dependency failing repeatedly.

### Benefit

Avoid cascading failures.

States:

```text
Closed
Open
Half-open
```

---

## Bulkhead

### Use When

Need failure isolation.

Example:

Separate pools for:

- Search
- Payments
- Notifications

---

## Graceful Degradation

### Goal

Serve partial functionality.

Example:

Amazon:

Recommendations fail.

Checkout still works.

---

# Storage Patterns

## Write Through Cache

Write:

```text
Cache
+
Database
```

Pros:

- Consistency

Cons:

- Slower writes

---

## Write Behind Cache

Write:

```text
Cache
first
```

Persist later.

Pros:

- Fast

Cons:

- Possible data loss

---

## Cache Aside

Most common.

Read:

```text
Cache
↓ miss
DB
↓
populate cache
```

---

# Idempotency

## Problem

Client retries request.

Payment executes twice.

---

## Solution

Idempotency Key

Example:

```http
Idempotency-Key:
abc123
```

Server remembers result.

---

# Availability Patterns

## Active-Passive

One leader.

One standby.

Pros:

- Simpler

Cons:

- Failover delay

---

## Active-Active

Multiple live instances.

Pros:

- Better availability

Cons:

- Hard consistency

---

# Search Systems

## Full Scan

Good for:

Small datasets.

---

## Inverted Index

Used by:

- Elasticsearch
- Lucene
- Solr

Maps:

```text
word -> documents
```

---

# Monitoring

## Golden Signals

Google SRE:

1. Latency
2. Traffic
3. Errors
4. Saturation

---

# Common Interview Questions

## CDN?

Use when:

- Static assets
- Global users

---

## Redis?

Use when:

- Hot reads
- Session storage
- Caching

---

## Kafka?

Use when:

- Event streaming
- Async processing
- Decoupling services

---

## SQL?

Use when:

- Transactions
- Joins
- Strong consistency

---

## NoSQL?

Use when:

- Massive scale
- Flexible schema
- High availability

---

# Quick Decision Table

| Problem | Typical Solution |
|-----------|----------------|
| Hot database | Cache |
| Hot cache key | Replication / sharding |
| Cache stampede | Single-flight lock |
| Massive reads | Read replicas |
| Massive writes | Sharding |
| Global latency | CDN |
| Traffic spikes | Auto scaling |
| API abuse | Rate limiting |
| Duplicate requests | Idempotency key |
| Failing dependency | Circuit breaker |
| Poison messages | DLQ |
| Network partition | CAP tradeoff |
| Rebalancing cluster | Consistent hashing |
| Need consensus | Raft |
| Event processing | Kafka |
| Strong transactions | SQL |
| Flexible high-scale data | NoSQL |
| Cross-service communication | Queue / Event Bus |
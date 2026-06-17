# Systems Design Cheat Sheet: Numbers, Vocabulary, Tradeoffs, and Resiliency

# Why Memorize These Numbers?

In system design interviews and real-world architecture discussions:

- Exact numbers are less important than order-of-magnitude reasoning.
- You should be able to estimate:
  - Requests per second (RPS)
  - Storage requirements
  - Network bottlenecks
  - Latency bottlenecks
  - Capacity planning
  - Cache hit rate impact

The goal is "back of the envelope" estimation.

---

# Latency Numbers Every Engineer Should Know

Approximate modern datacenter values.

| Operation | Latency |
|------------|----------|
| CPU register access | 0.3 ns |
| L1 cache | 1 ns |
| L2 cache | 4 ns |
| L3 cache | 10-20 ns |
| Main memory (RAM) | 100 ns |
| SSD random read | 100 μs |
| SSD sequential read | 10-50 μs |
| HDD seek | 5-10 ms |
| Local network (same rack) | 50-100 μs |
| Datacenter network | 0.5-1 ms |
| Cross-region AWS call | 20-100 ms |
| Cross-continent request | 100-300 ms |
| Human perception threshold | ~100 ms |
| Human notices delay | ~1 sec |

---

# Famous "Latency Numbers Every Programmer Should Know"

Convert to relative scale:

| Operation | Relative Cost |
|------------|---------------|
| RAM read | 1x |
| SSD read | 1,000x |
| Network call | 10,000x |
| Cross-region call | 100,000x |

Key lesson:

Network dominates almost everything.

---

# Bandwidth Numbers

## Typical Network Speeds

| Link | Bandwidth |
|--------|------------|
| Home internet | 100 Mbps - 1 Gbps |
| AWS ENA NIC | 10-100 Gbps |
| Datacenter backbone | 100-400 Gbps |
| Modern server NIC | 25-100 Gbps |

---

## Useful Conversions

### 1 Gbps

```text
≈ 125 MB/s
```

---

### 10 Gbps

```text
≈ 1.25 GB/s
```

---

### 100 Gbps

```text
≈ 12.5 GB/s
```

---

# Storage Numbers

## Typical Disk Sizes

| Storage | Size |
|----------|------|
| SSD | 1-8 TB |
| Enterprise SSD | 15-30 TB |
| S3 Bucket | Practically unlimited |

---

## Throughput

### SSD

Random:

```text
100k - 1M IOPS
```

Sequential:

```text
1-7 GB/s
```

---

### HDD

Random:

```text
100-300 IOPS
```

Sequential:

```text
100-250 MB/s
```

---

# CPU Throughput

This is workload-dependent.

---

## Simple API Server

Modern 4-8 vCPU instance:

```text
1,000 - 10,000 RPS
```

Typical CRUD API:

```text
2,000 - 20,000 RPS
```

---

## Nginx

```text
100,000+ RPS
```

Mostly limited by network.

---

## In-Memory Key-Value Store

Redis:

```text
100k - 1M+ ops/sec
```

Single node.

---

## Database

Postgres:

```text
1k-20k TPS
```

Depends heavily on workload.

---

# AWS EC2 Capacity Estimates

Very rough interview numbers.

---

## t3.medium

2 vCPU

```text
500 - 2000 RPS
```

---

## c7g.large

2 vCPU Graviton

```text
2,000 - 10,000 RPS
```

---

## c7g.4xlarge

16 vCPU

```text
20,000 - 100,000+ RPS
```

---

## Rule of Thumb

For a typical REST service:

```text
One medium EC2 instance
≈ several thousand requests/sec
```

Assume:

```text
2k-5k RPS
```

unless told otherwise.

---

# Database Numbers

## Postgres

Small queries:

```text
1-10 ms
```

Complex joins:

```text
10-100 ms
```

Bad query:

```text
100 ms - several seconds
```

---

## Redis

Typical:

```text
<1 ms
```

Often:

```text
100-500 μs
```

---

## DynamoDB

Single-digit millisecond latency.

Usually:

```text
1-10 ms
```

---

# Cache Numbers

## Redis

Read:

```text
<1 ms
```

---

## Memcached

```text
100-500 μs
```

---

## CDN Cache

```text
10-50 ms globally
```

---

# Availability Numbers

## Uptime Table

| Availability | Downtime / Year |
|-------------|----------------|
| 99% | 3.65 days |
| 99.9% | 8.7 hours |
| 99.99% | 52 minutes |
| 99.999% | 5 minutes |
| 99.9999% | 31 seconds |

---

# Capacity Planning Rules

## Daily Active Users → RPS

Rule:

```text
Average RPS

=
Daily Requests
/
86,400
```

---

Example

1 million users

10 requests/day

```text
10 million requests/day
```

Average:

```text
116 RPS
```

Peak:

```text
5x - 20x average
```

Plan for:

```text
500-2000 RPS
```

---

# Common Vocabulary

## Throughput

Amount of work completed per unit time.

Examples:

```text
5000 RPS
```

```text
100 MB/s
```

---

## Latency

Time to complete one operation.

Examples:

```text
p50 = 20 ms
p99 = 200 ms
```

---

## P50

Median request.

50% faster.

50% slower.

---

## P95

95% of requests faster than this.

---

## P99

99% of requests faster than this.

Most important production metric.

---

## Tail Latency

Slowest requests.

Often dominates user experience.

---

## SLA

Service Level Agreement.

Business promise.

Example:

```text
99.95% uptime
```

---

## SLO

Service Level Objective.

Internal reliability goal.

---

## SLI

Service Level Indicator.

Actual measured metric.

---

# Horizontal vs Vertical Scaling

## Vertical

Bigger machine.

```text
4 CPU → 32 CPU
```

Advantages:

- Simple

Disadvantages:

- Hard limit
- Expensive

---

## Horizontal

More machines.

```text
10 servers
→
100 servers
```

Advantages:

- Virtually unlimited

Disadvantages:

- More complexity

---

# CAP Theorem

Distributed systems can only strongly guarantee two:

| Property | Meaning |
|-----------|---------|
| Consistency | Everyone sees same data |
| Availability | System responds |
| Partition Tolerance | Survive network failures |

Since partitions are inevitable:

Real tradeoff:

```text
Consistency
vs
Availability
```

---

# Common Resiliency Components

## Load Balancer

Distributes traffic.

Examples:

- AWS ALB
- Nginx
- HAProxy

Benefits:

- Failover
- Scaling
- Health checks

---

## Auto Scaling

Automatically add/remove servers.

Benefits:

- Cost savings
- Traffic spikes

---

## Health Checks

Detect bad instances.

Examples:

```text
GET /health
```

---

## Retry

Retry failed requests.

Usually:

```text
3 attempts
```

with backoff.

---

## Exponential Backoff

Example:

```text
1 sec
2 sec
4 sec
8 sec
```

Prevents retry storms.

---

## Circuit Breaker

Stop calling failing dependency.

States:

```text
Closed
Open
Half-open
```

Benefits:

- Prevent cascading failure

---

## Bulkhead

Isolate resources.

Example:

Separate thread pools for:

```text
payments
search
recommendations
```

Failure in one doesn't kill others.

---

## Rate Limiting

Protect service.

Algorithms:

- Token Bucket
- Leaky Bucket
- Fixed Window
- Sliding Window

---

## Idempotency

Repeated request gives same result.

Example:

```text
Create payment
```

must not charge twice.

Often uses:

```text
Idempotency-Key
```

---

## Dead Letter Queue (DLQ)

Failed messages moved here.

Benefits:

- Prevent message loss
- Debug failures

---

## Message Queue

Examples:

- Kafka
- RabbitMQ
- SQS

Benefits:

- Decoupling
- Burst handling

---

# Common Database Scaling Patterns

## Read Replicas

One primary.

Many readers.

Good for:

```text
90% reads
10% writes
```

---

## Sharding

Split data.

Example:

```text
Users 1-1M
Users 1M-2M
Users 2M-3M
```

Advantages:

- Massive scale

Disadvantages:

- Operational complexity

---

# Cache Patterns

## Cache Aside

Most common.

Flow:

```text
Read cache
↓
Miss
↓
Read DB
↓
Populate cache
```

---

## Write Through

Write cache and DB together.

---

## Write Behind

Write cache first.

Persist later.

Higher performance.

More risk.

---

# Eventual Consistency

Updates propagate later.

Examples:

- DNS
- DynamoDB
- Cassandra

Tradeoff:

```text
Higher availability
Lower consistency
```

---

# Common Interview Bottlenecks

When system slows down, check:

1. Database
2. Network
3. Cache miss rate
4. Lock contention
5. CPU saturation
6. Memory pressure
7. Disk I/O
8. Queue backlog

Most systems fail because:

```text
Database becomes bottleneck
```

before application servers.

---

# Mental Math Numbers Worth Memorizing

## Time

```text
1 day = 86,400 sec
1 year ≈ 31.5 million sec
```

---

## Storage

```text
1 KB ≈ 10^3
1 MB ≈ 10^6
1 GB ≈ 10^9
1 TB ≈ 10^12
```

Interview approximations.

---

## Requests

```text
1000 RPS
≈
86 million requests/day
```

---

## Data Volume

```text
1 million users
× 1 KB profile
=
1 GB
```

---

## Log Volume

```text
1000 RPS
× 1 KB log
=
1 MB/sec

=
86 GB/day
```

---

# High-Level Design Workflow

1. Estimate traffic.
2. Estimate storage.
3. Estimate bandwidth.
4. Identify bottlenecks.
5. Add caching.
6. Add load balancing.
7. Add replication.
8. Add resiliency mechanisms.
9. Design monitoring.
10. Discuss tradeoffs.

For interviews, strong candidates rarely memorize exact AWS numbers. They know:
- Network is expensive.
- Databases bottleneck first.
- Cache hits are worth orders of magnitude.
- P99 matters more than average latency.
- Horizontal scaling beats vertical scaling long term.
- Reliability comes from redundancy, isolation, retries, and graceful degradation.
# HLD System Design Roadmap

Goal: FAANG / product-company backend interviews. Keep DSA as the main track (about 70% of time). HLD runs alongside it.

## How to use this file
- Work through the stages in order. Practice stages are marked **PRACTICE** and come early on purpose.
- Tick `` to `[x]` as you finish each item.
- Tier tags:  = master it,  = understand well enough to explain,  = learn later or when a problem needs it.
- "Done when" means you can actually do the thing, not just define it.

## Practice workflow (use for every design problem)
1. **Requirements:** functional, non-functional, scope, assumptions
2. **Estimation:** average QPS, peak QPS, read vs write ratio, storage (raw and replicated), bandwidth
3. **API design:** key endpoints, request/response, idempotency, auth
4. **Data model:** entities, keys, indexes, choose the DB from access patterns
5. **High-level diagram:** clients, LB, services, DB, cache, queue, external dependencies
6. **Deep dive:** pick 1-2 bottlenecks and fix them
7. **Failure modes:** retries, concurrent requests, node failure, data loss, degraded dependency
8. **Trade-offs:** what you chose, what you sacrificed (cost, latency, consistency, complexity)

Rules:
- First attempt of every problem: timebox to 45 minutes, on paper, **before** reading any solution.
- Then study the gaps and **redo the design from a blank page** later.
- Do not add Redis, Kafka, sharding, or microservices unless you can say what problem each one solves.
- Revisit earlier designs after learning new topics (e.g. URL shortener again after caching and sharding).

---

## STAGE 1: Foundations
-   How a request travels: DNS, TCP handshake, TLS, HTTP
-   HTTP/1.1 vs HTTP/2 vs HTTP/3; TCP vs UDP
-   WebSockets, long polling, SSE (revisit in the chat design)
-   Latency numbers: memory vs SSD vs network vs cross-region
-   Back-of-envelope estimation: QPS, peak vs average, storage per year, bandwidth, powers of 2 and 10
-   Metrics: latency (p50/p95/p99), throughput, availability ("nines"), durability
-   OS basics for backend: processes vs threads, context switching, memory, sockets, file descriptors
-   Concurrency vs parallelism (connect to your LLD multithreading videos)

**Done when:** for 100M users posting 2 times/day (about 1 KB each) you can compute average write QPS (about 2,300), peak QPS (about 23,000 at 10x), raw storage per day and per year (about 200 GB / 73 TB), then name the likely bottleneck and a fix, in under 2 minutes.

## STAGE 2: API fundamentals and security basics
-   REST: resources, verbs, status codes
-   gRPC and GraphQL; when to use each
-   Pagination: offset vs cursor; filtering; versioning
-   Idempotency and idempotency keys
-   Rate limiting algorithms: token bucket, leaky bucket, fixed window, sliding window log, sliding window counter
-   Distributed rate limiting (shared counters, Redis)
-   Webhooks; long-running jobs and async APIs (202 Accepted, polling, callbacks)
-   Authentication vs authorization
-   Sessions vs JWT
-   OAuth 2 flow
-   Basic abuse protection, input validation, API keys, avoiding sensitive-data exposure
-   HTTPS/TLS basics; password hashing (bcrypt/argon2, salts)

**Done when:** you can design a payment API that is safe to retry, and explain how it authenticates callers and limits abuse.

## STAGE 3: Database fundamentals
-   Data modeling: entities, relationships, constraints, normalization; design from access patterns first
-   SQL vs NoSQL: key-value, document, wide-column, graph; how to choose
-   Indexing: B-tree basics, composite indexes, covering indexes, cost of indexes
-   LSM-tree vs B-tree
-   Transactions and ACID
-   Isolation levels; dirty, non-repeatable, phantom reads
-   Concurrency control: locking vs MVCC, optimistic vs pessimistic locking
-   Race conditions: lost updates, double booking, preventing duplicates
-   Query optimization basics, connection pooling

**Done when:** you can model a booking system and show how two simultaneous requests for the last seat cannot both succeed.

## STAGE 4: Scaling the compute layer
-   Vertical vs horizontal scaling; why stateless services scale
-   Load balancers: L4 vs L7; algorithms (round robin, least connections, IP hash, weighted); health checks; sticky sessions
-   Load balancer high availability
-   Forward vs reverse proxy
-   API gateway: routing, auth, throttling; how it differs from an LB
-   Service discovery, autoscaling
-   Monolith vs microservices (trade-offs, when not to split)

**Done when:** you can explain why a session stored in server memory breaks horizontal scaling and give two fixes.

---

## STAGE 5: PRACTICE ROUND 1 (first designs)
Attempt each with only the knowledge from Stages 1-4. It will be imperfect. That is the point.
-  **URL shortener** (learn basics now: base62 encoding, counter vs hash vs random ID, collisions, 301 vs 302)
-  **Pastebin** (storage, expiry, partitioning)
-  **Rate limiter** (use the algorithms from Stage 2)
-  After each: list what you didn't know, and note which later stage fixes it

---

## STAGE 6: Caching and CDN
-   Where to cache: client, CDN, application, database
-   Patterns: cache-aside, read-through, write-through, write-back, write-around
-   Eviction: LRU, LFU, TTL
-   Pitfalls: invalidation, stampede, penetration, avalanche, hot keys
-   Redis vs Memcached; Redis data structures and use cases (why single-threaded Redis is fast)
-   CDN: push vs pull, edge caching, invalidation

**Done when:** you can explain what happens when a very popular cache key expires under heavy load, and three ways to prevent the stampede.

## STAGE 7: Database scaling
-   Replication: leader-follower, synchronous vs asynchronous, replication lag, failover
-   Multi-leader and leaderless replication
-   Read replicas; read-your-writes problem
-   Partitioning/sharding: range vs hash, shard key choice
-   Consistent hashing, virtual nodes
-   Hot keys/hot shards, rebalancing, cross-shard queries and joins
-   Denormalization trade-offs
-   Quick look: time-series DBs, columnar/OLAP, data lakes

**Done when:** you can say how you'd shard a users table, choose the shard key, and list what breaks when you do.

## STAGE 8: PRACTICE ROUND 2 (revisit and extend)
-  **Redo URL shortener** at scale (caching, replication, sharding, estimates for billions of URLs)
-  **Key-value store** (partitioning, replication, consistent hashing; deepen after Stage 11)
-  **Distributed rate limiter** version (shared state, race conditions)

---

## STAGE 9: Async processing and messaging
-   Why queues: decoupling, buffering, retries
-   Queue vs pub/sub vs log
-   Kafka: topics, partitions, consumer groups, offsets, retention, why it's fast
-   RabbitMQ/SQS vs Kafka
-   Delivery semantics: at-most-once, at-least-once, "effectively once" via idempotency; why exactly-once is hard
-   Dead-letter queues, ordering guarantees, backpressure
-   Batch vs stream processing (high level)
-   Outbox pattern

**Done when:** you can design a notification pipeline that retries failures without sending duplicates.

## STAGE 10: PRACTICE ROUND 3 (queues, fan-out, real-time)
Learn these real-time topics alongside the problems:
-   Fan-out on write vs fan-out on read (and hybrid for celebrities)
-   WebSocket scaling: connection servers, routing messages to the right server
-   Presence (online/offline status) and push vs pull delivery
-   Message ordering and delivery receipts

Problems:
-  **Notification system** (queues, retries, idempotency)
-  **News feed / Twitter** (fan-out, caching, sharding, celebrity problem)
-  **Chat system (WhatsApp)** (WebSockets, ordering, delivery receipts, presence)

---

## STAGE 11: Distributed systems theory
-   CAP and PACELC, and what they actually mean
-   Consistency models: strong, eventual, causal, read-your-writes
-   Quorums (R + W > N)
-   Consensus: the idea behind Raft/Paxos, leader election, ZooKeeper/etcd use cases
-   Clocks: why wall clocks are unreliable; logical clocks; vector clocks
-   Distributed transactions: 2PC, saga pattern
-   Gossip protocol, failure detection
-   Multi-leader conflict resolution, CRDTs (names only)

**Done when:** you can explain what eventual consistency costs a user and when it's acceptable.

## STAGE 12: Reliability and operations
-   Timeouts, retries with exponential backoff and jitter
-   Circuit breakers, bulkheads
-   Graceful degradation, load shedding
-   Redundancy, single points of failure; replication across zones/regions
-   Observability: logs, metrics, traces; alerting; SLI/SLO/SLA
-   Deployment strategies: canary, blue-green, rolling; feature flags
-   Disaster recovery: backups, RTO, RPO (know the terms)
-   Capacity and cost estimation: replicas, request size, egress, cost vs performance trade-offs
-   Multi-tenancy and isolation (SaaS systems)

**Done when:** you can walk through how a system survives one dependency going down.

## STAGE 13: Specialized building blocks
-   Unique ID generation: UUID, Snowflake, ticket servers
-   Geospatial: geohash, quadtree
-   Search: inverted index, Elasticsearch basics
-   Autocomplete: trie, caching top suggestions
-   Object storage (S3), file chunking, deduplication, resumable uploads
-   Event-driven architecture, CQRS (when justified)
-   Probabilistic structures: Bloom filter (know it), HyperLogLog, count-min sketch
-   Service-to-service security, secrets management

---

## STAGE 14: PRACTICE ROUND 4 (harder designs)
Roughly in this order:
-  **Typeahead/autocomplete** (tries, caching)
-  **Ticket booking** (concurrency, double-booking, holds with expiry)
-  **Web crawler** (queues, politeness, dedup, bloom filter)
-  **YouTube/Netflix** (object storage, CDN, transcoding pipeline)
-  **Dropbox/Google Drive** (chunking, sync, dedup, conflict handling)
-  **Uber** (geospatial index, matching, location updates)
-  **Payment system** (idempotency, ledger, reconciliation, exactly-once effects)
-  **Distributed cache** (consistency, eviction, replication, hot keys)
-  **Ad click aggregator** (streaming, windows, exactly-once counting)
-  **Metrics/monitoring system** (time-series, ingestion, alerting)

## STAGE 15: Interview readiness (ongoing)
-  Do 3+ timed, spoken, 45-minute sessions on paper or a whiteboard tool
-  Do at least 3 mock interviews (friend, peer, or AI as a pushy interviewer)
-  Re-do your 5 weakest designs from a blank page
-  Write a one-page cheat sheet: estimation numbers, common patterns, trade-off phrases
-  Read 3-5 engineering blog posts (Uber, Netflix, Discord, Cloudflare, Meta) and compare to your designs

---

## Resources (mostly free)
- System Design Primer (GitHub, donnemartin): topic map
- Hello Interview: written problem breakdowns
- ByteByteGo and Gaurav Sen (YouTube): concept and problem videos
- Jordan Has No Life (YouTube): deeper designs
- Martin Kleppmann's distributed systems lectures (YouTube); MIT 6.824 lectures (heavier)
- DDIA (book, paid, library copy works): Ch. 3, 5, 6, 7, 9 are the most valuable
- Papers (read intro, skim rest): Dynamo, Kafka, Bigtable, GFS, Raft
- Engineering blogs: Uber, Netflix, Discord, Cloudflare, Meta; highscalability.com

## Using AI as a tutor
- Ask it to quiz you ("ask me 5 hard questions on sharding, one at a time, critique my answers").
- Paste your own design and ask for bottlenecks and failure modes.
- Verify numbers and fine details against primary sources; do not copy finished designs.

## Optional small projects (only if time allows next to DSA)
- Redis-backed rate limiter
- Idempotent payment endpoint
- Background job worker with retries
- Cursor-paginated API

## Time estimate
About 10-14 weeks at 6-8 hours/week alongside DSA. Minimum viable set for 2-year-experience interviews: Stages 1-10 plus Stage 12 basics.

## Progress log
| Date | Stage | Notes / weak spots |
|---|---|---|
| | | |

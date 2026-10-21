# Introduction to HLD (High Level Design)

## What is it?
HLD is the big picture of a system: which major components exist and how they work together to serve many users reliably. Think of planning a city: roads, power, water, and how they connect, not the layout of each house.

**Core question:** "How do I build X so it works for millions of users and keeps working when things fail?"

## What you actually decide
- Which components to use: servers, load balancers, databases, caches, queues, CDN
- How data is stored, split, and copied
- How requests flow through the system
- How the system handles huge traffic, slow parts, and failures
- What trade-offs you accept (speed vs consistency, cost vs reliability)

## Why it exists (the problem it solves)
One server and one database work fine for 100 users. At millions of users you hit limits:
- One machine can't handle all the traffic -> **scaling, load balancing**
- One database becomes slow or fails -> **replication, sharding**
- Repeated reads are slow -> **caching**
- Slow tasks block users -> **queues and async processing**
- Machines crash and networks fail -> **reliability patterns**

HLD is the set of ideas for solving these problems.

## Simple example: a tiny app growing up
1. **Start:** client -> one server -> one database
2. **More users:** add a load balancer and several servers
3. **Slow reads:** add a cache (Redis)
4. **Database too big:** replicate it, then shard it
5. **Slow background work:** add a queue (Kafka/SQS) and workers
6. **Global users:** add a CDN
7. **Failures:** add timeouts, retries, monitoring

## What an HLD interview looks like (about 45 minutes)
1. Clarify requirements (functional and non-functional)
2. Estimate scale (QPS, storage, bandwidth)
3. Define APIs and the data model
4. Draw the high-level architecture
5. Deep dive into one or two bottlenecks
6. Discuss failures and trade-offs

You are judged on your reasoning and trade-offs, not on memorizing "the right answer".

## Key vocabulary (you will learn all of these)
- **Latency:** time for one request. **Throughput:** requests handled per second.
- **QPS:** queries per second.
- **Availability:** how much of the time the system works ("nines").
- **Scalability:** ability to handle growth.
- **Consistency:** whether all users see the same, up-to-date data.
- **Load balancer, cache, shard, replica, queue, CDN.**

## How this folder is organized
- `HLD-System-Design-Roadmap.md`: the stage-by-stage checklist. Start at Stage 1.
- `concepts/`: one note per topic (caching, sharding, Kafka, ...)
- `designs/`: one note per practice problem (URL shortener, news feed, ...)
- `diagrams/`: your drawings

## Prerequisites
Basic networking (HTTP, DNS), basic databases (SQL), and one backend language. You do not need to know everything first. The roadmap fills gaps as you go.

## Golden rules
- Start simple. A single server and database is a valid first design.
- Add a component only when you can say what problem it solves.
- Every choice has a cost. Always state the trade-off.
- Practice designing early, then revisit the same designs as you learn more.

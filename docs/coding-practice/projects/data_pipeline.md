---
title: Data Pipeline
parent: Projects
nav_order: 3
toc: true
---

# High-Frequency Crypto Market Data Pipeline
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview

**Goal:** Build a production-quality, low-latency multi-exchange cryptocurrency market data pipeline in Java, applying distributed systems principles from "Designing Data-Intensive Applications" (DDIA).

**Time Commitment:** 40-50 hours/week (full-time intensity)

**End Result:** Portfolio project demonstrating concurrency, distributed systems, stream processing, and performance engineering - core skills for quantitative developer roles.

---

## Learning Philosophy

### DDIA Integration Strategy
- Read chapters **just-in-time** before you need them
- Apply concepts **immediately** to your pipeline
- Use DDIA to **justify architectural decisions**
- Revisit chapters as understanding deepens

### Build-While-Learning Approach
- No separation between "learning" and "building" phases
- Implement concepts immediately after learning
- Iterate fast, refactor later
- Measure performance from day one

---

## Week 1-2: Java Foundations + Core Data Structures

### Focus: Language Basics → CS61B Level

**Java Fundamentals (Days 1-3)**
- Syntax: variables, control flow, methods, arrays
- OOP: classes, objects, inheritance, polymorphism, interfaces
- **Resource:** Java Tutorial for Beginners (Mosh) at 1.5x speed
- **Practice:** 20 LeetCode Easy problems

**Core Data Structures (Days 4-14)**
- Implement from scratch: ArrayList, LinkedList, HashMap
- Study: Binary Search Trees, Heaps, Priority Queues
- Master: Java Collections Framework (List, Set, Map, Queue)
- Learn: Streams API, lambda expressions, functional interfaces
- **Resource:** CS61B Lectures 1-30 (watch at 2x, skip Gitlet project)
- **Build:** Order book prototype using TreeMap for sorted price levels

**Key Skills Acquired:**
- Fluency with Java syntax and Collections Framework
- Big-O analysis and algorithm complexity
- When to use which data structure
- Clean, tested Java code

**Checkpoint:** Can you implement a HashMap from scratch and explain time complexity trade-offs?

---

## Week 3: Concurrency Fundamentals

### Focus: Thread-Safe Code + DDIA Chapter 7 (Transactions)

**Threading Basics (Days 1-3)**
- Thread, Runnable, ExecutorService
- synchronized keyword, volatile, race conditions, deadlocks
- Thread pools and coordination patterns
- **Resource:** Java Concurrency in Practice Ch 1-3, Jenkov tutorials

**Concurrent Collections (Days 4-7)**
- ConcurrentHashMap, ConcurrentSkipListMap, BlockingQueue
- AtomicInteger, AtomicReference, compare-and-swap (CAS)
- Lock-free programming concepts
- **Resource:** Java Concurrency in Practice Ch 5, 14-15

**DDIA Chapter 7: Transactions**
- **Read:** Atomicity, isolation levels, lost update problem
- **Apply:** Understand order book consistency model
  - Atomic updates per price level (CAS operations)
  - Read-committed isolation (no global locks)
  - Why serializable isolation would kill throughput
- **Document:** Consistency guarantees in your README

**Build This Week:**
- Multi-threaded order book simulator
- 3 producer threads generating updates → 1 consumer maintaining state
- Compare: synchronized vs ConcurrentSkipListMap performance
- Implement object pooling to reduce GC pressure
- Measure throughput (updates/second)

**Key Skills Acquired:**
- Write thread-safe code confidently
- Choose appropriate concurrent data structures
- Understand lock contention and performance implications

**Checkpoint:** Can you explain why ConcurrentSkipListMap is better than synchronized TreeMap for your use case?

---

## Week 4: Network Programming + Kafka

### Focus: Distributed Systems + DDIA Chapter 8 (Trouble with Distributed Systems)

**WebSocket Client (Days 1-3)**
- Java WebSocket libraries (Java-WebSocket)
- JSON parsing with Jackson
- Async I/O patterns
- **Build:** Connect to Binance/Coinbase WebSocket, parse order book updates

**DDIA Chapter 8: Distributed Systems**
- **Read:** Network partitions, timeouts, clock synchronization, partial failures
- **Apply:** Robust connection handling
  - Exponential backoff for reconnection
  - Heartbeat to detect silent failures
  - Use local receive timestamps (not exchange timestamps)
  - Never trust network or clocks
- **Document:** Failure modes and mitigation strategies

**Kafka Integration (Days 4-7)**
- Kafka concepts: topics, partitions, consumer groups
- Producer/Consumer API
- Offset management and exactly-once semantics
- **Setup:** Docker-based Kafka locally
- **Resource:** Kafka Quickstart, Java Client documentation

**Build This Week:**
- WebSocket client with reconnection logic and heartbeat
- Producer: WebSocket → Kafka topic
- Consumer: Kafka → in-memory order book
- Measure end-to-end latency (message received → book updated)

**Key Skills Acquired:**
- Network programming with WebSocket
- Kafka producer/consumer patterns
- Handling distributed system failures
- Understanding clock skew and timing issues

**Checkpoint:** Can your system recover automatically from network partitions and Kafka broker restarts?

---

## Week 5: Multi-Exchange Architecture

### Focus: Aggregation + DDIA Chapter 5 (Replication) & Chapter 11 (Stream Processing)

**DDIA Chapter 5: Replication**
- **Read:** Multi-leader replication, conflict resolution, replication lag
- **Apply:** Multi-exchange aggregation strategy
  - Each exchange is an independent "leader"
  - NBBO calculation: best-available (max bid, min ask)
  - Consolidated depth: sum quantities (conflict resolution)
  - Staleness detection (exclude data >1ms old)
- **Document:** Multi-leader pattern in your architecture

**DDIA Chapter 11: Stream Processing**
- **Read:** Event sourcing, stateful stream processing, exactly-once semantics
- **Apply:** Core architectural pattern
  - Kafka = immutable event log (source of truth)
  - Order book = materialized view derived from log
  - Partition by symbol for ordering guarantees
  - Idempotent consumers (safe to replay)
  - Can rebuild state by replaying Kafka
- **Document:** Event sourcing benefits (fault tolerance, debugging, backtesting)

**Build This Week:**
- WebSocket clients for 3 exchanges (Binance, Coinbase, Kraken)
- Unified message format and Kafka topic structure
- Order book manager aggregating all exchanges
- NBBO calculator with staleness checks
- Market depth aggregation (5/10/20 levels)

**Key Skills Acquired:**
- Multi-source data aggregation
- Conflict resolution strategies
- Event sourcing architecture
- Stream processing patterns

**Checkpoint:** Does your NBBO calculation exclude stale data and handle missing exchange feeds gracefully?

---

## Week 6: Performance Optimization

### Focus: Low Latency + DDIA Chapter 1 (Performance Metrics)

**DDIA Chapter 1: Reliable, Scalable, Maintainable Applications**
- **Read:** Latency vs throughput, percentiles (p50/p99/p99.9), SLOs
- **Apply:** Performance measurement strategy
  - Use HdrHistogram for accurate tail latency
  - Define SLOs (e.g., p99 < 500μs)
  - Report percentiles, not averages
  - Tail latency matters more than average
- **Document:** Performance characteristics and SLOs

**Optimization Work (Days 1-5)**
- Profile with VisualVM/JProfiler
- Identify hotspots (likely: object allocation, GC pauses)
- Implement object pooling for update messages
- Optimize memory layout for cache locality
- Tune JVM: heap size, GC algorithm selection
- Benchmark with JMH (Java Microbenchmark Harness)

**Storage Layer (Days 6-7)**
- Redis: in-memory snapshots of current order book state
- ClickHouse (or CSV): historical time-series data
- Simple REST API to query current state

**Build This Week:**
- Comprehensive latency measurement infrastructure
- Object pooling implementation
- JVM tuning for low latency
- Storage layer for snapshots and historical data
- Before/after benchmarks showing improvements

**Key Skills Acquired:**
- Profiling and optimization techniques
- JVM performance tuning
- Tail latency analysis
- Storage engine trade-offs

**Checkpoint:** Can you demonstrate p99 latency under 500μs and explain what optimizations achieved this?

---

## Week 7: Production Quality

### Focus: Operational Excellence + DDIA Chapter 12 (Future of Data Systems)

**DDIA Chapter 12: Future of Data Systems**
- **Read:** Observability, derived data, data integration, unbundling databases
- **Apply:** Production operational patterns
  - Metrics: Prometheus format (message rate, latency, errors)
  - Structured logging: JSON for machine parsing
  - Health checks: staleness, consumer lag, connection status
  - Graceful shutdown handling
- **Document:** Your system as a "specialized database" for market data

**Production Practices (Days 1-7)**
- Comprehensive logging with SLF4J/Logback
- Metrics and monitoring (Prometheus-compatible)
- Unit tests: order book operations, message parsing
- Integration tests: full pipeline end-to-end
- Load tests: 10,000 updates/sec sustained
- Test coverage with JaCoCo
- CI/CD: GitHub Actions for test + build
- Docker containerization with docker-compose
- Configuration via environment variables
- Professional README with architecture diagrams

**Build This Week:**
- Complete test suite (unit + integration + load)
- Metrics and monitoring infrastructure
- Dockerized deployment
- CI/CD pipeline
- Production-ready documentation

**Key Skills Acquired:**
- Testing strategies (unit/integration/load)
- Observability (logs, metrics, traces)
- Containerization and deployment
- CI/CD automation

**Checkpoint:** Can someone else clone your repo, run `docker-compose up`, and have a working pipeline in 5 minutes?

---

## Week 8: Advanced Topics + Polish

### Focus: Storage & Schema + DDIA Chapters 3 & 4

**DDIA Chapter 3: Storage and Retrieval**
- **Read:** Log-structured vs B-tree storage, in-memory databases, column-oriented storage
- **Apply:** Storage layer decisions
  - Kafka: log-structured, append-only
  - Redis: in-memory (fast, volatile)
  - ClickHouse: columnar (analytics-optimized)
- **Document:** Why each storage engine for each use case

**DDIA Chapter 4: Encoding and Evolution**
- **Read:** Schema evolution, binary encoding (Avro, Protocol Buffers)
- **Apply:** Kafka message schema with Avro
  - Forward/backward compatibility
  - Can add fields without breaking consumers
- **Document:** Schema evolution strategy

**Final Polish (Days 1-7)**
- Code cleanup and refactoring
- Performance tuning final pass
- Add monitoring dashboard (optional)
- Technical blog post or architecture writeup
- Demo video or detailed screenshots
- Performance benchmark documentation

**Optional: Start Python Trading System**
- Begin event-driven backtester project
- Portfolio state management
- Strategy framework
- This provides Python/Java contrast for resume

**Key Skills Acquired:**
- Storage engine trade-offs
- Schema design and evolution
- System documentation
- Technical communication

**Checkpoint:** Is your project portfolio-ready with clear documentation, benchmarks, and architecture diagrams?

---

## Week 9-10: Buffer + Interview Prep

### Choose Your Path

**Option A: Advanced Features (If Ahead of Schedule)**
- DDIA Chapter 6 (Partitioning): horizontal scaling by symbol
- DDIA Chapter 9 (Consistency): understand why eventual consistency suffices
- DDIA Chapter 10 (Batch Processing): backtest framework using Kafka replay
- Trade matching logic
- Advanced analytics (volatility calculation, order flow imbalance)
- Multi-timeframe aggregation (1s, 1m, 5m bars)

**Option B: Interview Preparation (Recommended)**
- Review common Java interview questions
- Practice explaining your architecture decisions
- Mock system design interviews
- Prepare DDIA-based answers for design questions
- Create presentation about your pipeline
- Study concurrency patterns and pitfalls

**Option C: Breadth (Exploring Adjacent Skills)**
- Learn C++ basics (syntax, pointers, STL)
- Convert one component to C++ for comparison
- Start Auto Alpha Mining project in Python
- Explore FPGA/hardware acceleration concepts
- Study other HFT system architectures

**Build This Week:**
- Polished final presentation of your work
- Architecture decision records (ADRs)
- Comprehensive performance analysis
- Comparison with industry systems
- Future enhancement roadmap

---

## Final Deliverables

### GitHub Repository Should Include:

**Code**
- Production-quality Java implementation
- Comprehensive test suite
- Docker deployment configuration
- CI/CD pipeline setup

**Documentation**
- README with architecture overview and DDIA references
- Setup and deployment instructions
- Performance benchmarks and analysis
- Design decisions document

**Demonstrations**
- Latency measurements (p50/p99/p99.9)
- Throughput benchmarks
- Fault tolerance demonstrations
- Multi-exchange aggregation examples

---

## DDIA Reading Schedule

| Week | Chapter | Focus | Application |
|------|---------|-------|-------------|
| 3 | Ch 7: Transactions | Consistency models | Order book atomicity |
| 4 | Ch 8: Distributed Systems | Failure modes | Network resilience |
| 5 | Ch 5: Replication | Multi-leader patterns | Multi-exchange aggregation |
| 5 | Ch 11: Stream Processing | Event sourcing | Core architecture |
| 6 | Ch 1: Performance | Metrics | Latency measurement |
| 7 | Ch 12: Future of Data | Observability | Production operations |
| 8 | Ch 3: Storage | Storage engines | Redis vs ClickHouse |
| 8 | Ch 4: Encoding | Schema evolution | Avro for Kafka |
| 9-10 | Ch 6: Partitioning | Horizontal scaling | Symbol partitioning |
| 9-10 | Ch 9: Consistency | Consensus | Why eventual consistency |
| 9-10 | Ch 10: Batch Processing | Historical analysis | Backtesting framework |

**Skip:** Chapters 2 (Data Models), 13+ (not immediately relevant)

---

## Interview Preparation: DDIA-Backed Answers

### "Why Kafka instead of direct WebSocket to order book?"

*"I followed the event sourcing pattern from DDIA Chapter 11. Kafka provides an immutable log of all market events, enabling fault tolerance through replay, debugging by replaying specific time windows, and backtesting using the same code path. Without Kafka, I'd lose events during restarts and couldn't backtest strategies."*

### "How do you handle multiple exchanges with conflicting data?"

*"It's the multi-leader replication problem from DDIA Chapter 5. Each exchange is an independent leader. For NBBO, I use best-available conflict resolution. For consolidated depth, I sum quantities at matching prices. I exclude stale data (>1ms old). This is eventual consistency, which DDIA Chapter 9 explains is appropriate when strong consistency would kill latency."*

### "Why measure p99 instead of average latency?"

*"DDIA Chapter 1 emphasizes that percentiles matter more than averages. My average might be 200μs, but p99 at 5ms due to GC is what users experience. I use HdrHistogram for accurate tail latency measurement. After profiling, I reduced p99.9 from 2ms to 600μs through object pooling."*

### "What happens if your consumer crashes?"

*"Thanks to event sourcing (DDIA Ch 11), Kafka retains all events. The consumer rebuilds order book state by replaying from its last committed offset. This is like rebuilding a materialized view from a log. The system self-heals with no data loss. I could even rebuild state from 3 days ago by seeking to that timestamp."*

### "Why not use a traditional database?"

*"DDIA Chapter 12 discusses 'unbundling databases' - building specialized systems for specific workloads. Traditional databases optimize for ACID and complex queries. My order book needs microsecond updates with minimal locking. I built a specialized in-memory structure (ConcurrentSkipListMap) that's 100x faster than Postgres for this specific use case."*

---

## Success Metrics

### Technical Competence
- ✅ P99 latency < 500μs for order book updates
- ✅ 10,000+ updates/sec throughput per symbol
- ✅ Automatic recovery from network failures
- ✅ Zero data loss during restarts
- ✅ 80%+ test coverage

### Systems Understanding
- ✅ Can explain every architectural decision with DDIA backing
- ✅ Understand trade-offs between consistency models
- ✅ Know when to use which storage engine
- ✅ Can discuss failure modes and mitigation strategies

### Professional Skills
- ✅ Production-quality code (tested, logged, monitored)
- ✅ Clear documentation with diagrams
- ✅ Containerized deployment
- ✅ CI/CD automation
- ✅ Performance analysis and benchmarking

---

## Common Pitfalls to Avoid

**DON'T:**
- Read DDIA cover-to-cover before coding
- Skip fundamentals (data structures, concurrency)
- Over-engineer with microservices
- Ignore performance until the end
- Build everything from scratch (use libraries)

**DO:**
- Read DDIA chapters just-in-time
- Build incrementally with working prototypes
- Measure latency from day one
- Write tests as you go
- Document design decisions immediately

---

## Post-Project: What's Next?

### Immediate Applications
- Apply to quant dev roles at prop trading firms
- Interview confidently with systems design examples
- Build complementary Python projects (backtesting, alpha mining)
- Explore C++ for ultra-low-latency systems

### Skill Extensions
- FPGA programming for hardware acceleration
- Options pricing and Greeks calculation
- Machine learning for market prediction
- Cross-asset arbitrage strategies

### Career Positioning
- Portfolio demonstrates: concurrency, distributed systems, performance engineering
- Can discuss: event sourcing, stream processing, consistency models
- Competitive for: Millennium, Citadel, Jane Street, Two Sigma, DRW, Virtu
- Skills transfer across: equities, FX, crypto, fixed income

---

## Time Investment Summary

**Full-Time (40-50 hrs/week): 10 weeks**
- Weeks 1-2: Java foundations
- Week 3: Concurrency
- Week 4: Networking + Kafka
- Week 5: Multi-exchange architecture
- Week 6: Performance optimization
- Week 7: Production quality
- Week 8: Polish + advanced topics
- Weeks 9-10: Buffer + interview prep

**Part-Time (15-20 hrs/week): ~20 weeks**
- Double the timeline
- Focus on core functionality (skip optional features)
- Minimum viable product by week 14

---

## Bottom Line

This roadmap transforms you from **Java beginner** to **production systems engineer** in 10 weeks, with architectural knowledge from DDIA that separates you from candidates who just code.

**You'll build:** A portfolio-quality distributed system demonstrating low-latency programming, stream processing, and operational excellence.

**You'll understand:** Why event sourcing, what consistency model to choose, how to measure performance, when to optimize.

**You'll be ready for:** Quantitative developer interviews at top-tier trading firms, with concrete examples of distributed systems you've built and DDIA-backed explanations for every design decision.
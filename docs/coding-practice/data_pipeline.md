---
title: Market Data Pipeline
parent: Coding Camp
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

## Objectives

- Learn high performance computing with Java
- Learn Kafka
- Good quant dev project to put on resume
- Can be a generic tool for later self-trading in crypto

---

## Definition of Done

### Core Functionality
- [ ] Connect to 3+ crypto exchanges (Binance, Coinbase, Kraken) via WebSocket
- [ ] Reconstruct L2 order books with accurate state management
- [ ] Calculate cross-exchange best bid/offer (NBBO)
- [ ] Match trades to order book snapshots
- [ ] Support historical data replay for backtesting

### Performance Targets
- [ ] End-to-end latency: P99 <50ms (exchange → storage)
- [ ] Order book updates: P99 <5ms in Redis
- [ ] Process >100k messages/second sustained
- [ ] Query 1-day data in <100ms (ClickHouse)

### System Quality
- [ ] 99.9% uptime with automatic failover
- [ ] Zero message loss on restarts
- [ ] >80% test coverage with integration tests
- [ ] Monitoring dashboard with latency/throughput metrics

### Production Ready
- [ ] Dockerized services deployed on AWS
- [ ] CI/CD pipeline with automated tests
- [ ] Structured logging and alerting
- [ ] Disaster recovery tested (<1hr RTO)

---

## Technical Approach

### Phase 1: SQL Fundamentals and Database Design
**Duration**: 2-3 weeks  
**Status**: In Progress

[`./databases`](./csdiy/databases)

**Deliverables**:
- Understanding of relational data modeling
- Proficiency in complex queries (joins, aggregations, window functions)
- Knowledge of indexing strategies and query optimization
- Basic understanding of OLAP vs OLTP databases


### Phase 2: Foundation - Data Models and Storage Layer
**Duration**: 2-3 weeks  
**Focus**: DDIA Chapters 1-4

**Objectives**

Design optimal schemas for crypto market data and set up storage infrastructure.

**Key Tasks**

- Design normalized/denormalized schemas for ticks, trades, order books
- Deploy ClickHouse with MergeTree engines, partitioning by date/exchange
- Set up Redis cluster with order book data structures (sorted sets)
- Evaluate and implement message serialization (Protobuf/Avro/MessagePack)
- Benchmark performance and document tradeoffs

**Deliverables**

- Operational ClickHouse + Redis clusters
- Message serialization library with benchmarks
- Schema documentation

**Success Metrics**

- ClickHouse: >10k inserts/sec, 1-day queries <500ms
- Redis: P99 read latency <1ms
- Serialization: <100μs per message


### Phase 3: Distributed Systems - Kafka Streaming Architecture
**Duration**: 3-4 weeks  
**Focus**: DDIA Chapters 5-9

**Objectives**
Build end-to-end streaming pipeline from exchange WebSockets to normalized data storage.

**Key Tasks**
- Deploy Kafka cluster with appropriate partitioning (exchange+symbol hash)
- Build Python async WebSocket connectors for Binance, Coinbase, Kraken
- Implement Java Kafka consumers for message normalization
- Build high-performance order book engine with concurrent data structures
- Implement exactly-once processing with Kafka transactions
- Add automatic reconnection, deduplication, error handling

**Deliverables**
- Production Kafka cluster
- WebSocket connectors for 3+ exchanges
- Order book engine with Redis snapshots
- Exactly-once processing with tests

**Success Metrics**
- Consumer lag <1 second under peak load
- Order book update: P99 <5ms
- Process >100k messages/sec
- Zero message loss on restarts

### Phase 4: Stream Processing and Analytics
**Duration**: 2-3 weeks  
**Focus**: DDIA Chapters 10-11

**Objectives**
Implement stream processing patterns for real-time analytics and cross-exchange aggregation.

**Key Tasks**
- Implement windowing operations and stateful stream processing
- Build NBBO calculation engine consuming from all exchanges
- Create trade matching pipeline (match trades to order book state)
- Implement batch processing for historical reconstruction
- Add market depth analytics and liquidity metrics

**Deliverables**
- Stream processing applications with aggregations
- NBBO engine streaming updates to storage
- Trade matching pipeline
- Batch backfill framework

**Success Metrics**
- NBBO calculation: P99 <2ms
- Trade matching: 100% accuracy
- Handle late data up to 5 seconds
- Batch: process 1 day in <10 minutes
 

### Phase 5: Production Readiness
**Duration**: 2-3 weeks  
**Focus**: DDIA Chapter 12

**Objectives**
Make system production-grade with monitoring, reliability, and operational excellence.

**Key Tasks**
- Set up Prometheus/Grafana for metrics and dashboards
- Configure alerting for critical failures
- Profile and optimize hot paths (JVM tuning, query optimization)
- Implement circuit breakers, retry logic, graceful degradation
- Build schema evolution with registry
- Test disaster recovery and document runbooks

**Deliverables**
- Monitoring stack with dashboards
- Alert system with runbooks
- Performance optimization report
- Disaster recovery procedures

**Success Metrics**
- Uptime: >99.9%
- MTTD <2 minutes, MTTR <15 minutes
- >30% latency improvement from profiling
- Full recovery in <1 hour


### Phase 6: Advanced Features (Optional)
**Duration**: 3-4 weeks  
**Focus**: Extended capabilities

**Objectives**
Add ML integration, advanced analytics, and research-friendly interfaces.

**Key Tasks**
- Build feature store for ML pipelines
- Implement market microstructure analytics (order flow, toxicity metrics)
- Create FastAPI layer for queries and backtesting
- Add GPU acceleration evaluation for feature computation

**Deliverables**
- Feature store integrated with model training
- REST API with authentication
- Backtesting framework with replay
- Advanced optimization research

---

## Timeline Summary

| Phase | Duration | Key Milestone |
|-------|----------|--------------|
| Phase 1: SQL Fundamentals | 2-3 weeks | Database design proficiency |
| Phase 2: Data Models & Storage | 2-3 weeks | ClickHouse + Redis operational |
| Phase 3: Kafka Architecture | 3-4 weeks | End-to-end streaming pipeline |
| Phase 4: Stream Processing | 2-3 weeks | NBBO and analytics live |
| Phase 5: Production Readiness | 2-3 weeks | Monitoring and reliability |
| Phase 6: Advanced Features | 3-4 weeks | ML integration and APIs |
| **Total (Core)** | **11-16 weeks** | **Phases 1-5 complete** |
| **Total (Extended)** | **14-20 weeks** | **All phases complete** |

---

## Risk Mitigation

**Technical**: Continuous profiling, Kafka replication, adapter pattern for exchange APIs  
**Operational**: Incremental development, dedicated DDIA study time, strict scope management

---

## Success Criteria

Project complete when:
1. Phases 1-5 deliverables meet all success metrics
2. Definition of Done is 100% satisfied
3. System demonstrates >99.9% uptime under load
4. Documentation enables independent deployment

Demonstrates understanding of distributed systems, production infrastructure, and technologies needed for quant dev roles.

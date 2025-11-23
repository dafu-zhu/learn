---
title: Trading System
parent: Projects
nav_order: 1
---

# Event-Driven Trading System with Backtesting
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

**Project Start Date:** November 23, 2025  
**Progress:** <span id="progress">Loading...</span>


Github repo: [`./trading-system`](https://github.com/dafu-zhu/trading-system)

## Project Overview

Build a production-grade event-driven trading system fulfilling FINM 32500 course requirements while demonstrating institutional-quality architecture, real-time data processing, and deployment capabilities.

**Project Start Date:** November 23, 2025  
**Progress:** <span id="progress">Loading...</span>

<script>
window.addEventListener('load', function() {
  const projectStart = new Date(2025, 10, 23);
  const today = new Date();
  const diffMs = today - projectStart;
  const diffDays = Math.floor(diffMs / (1000 * 60 * 60 * 24)) + 1;
  if (diffDays < 1) {
    document.getElementById('progress').textContent = 'Not started yet';
  } else {
    const weekNum = Math.ceil(diffDays / 7);
    const dayInWeek = ((diffDays - 1) % 7) + 1;
    document.getElementById('progress').textContent = `Week ${weekNum}, Day ${dayInWeek}`;
  }
});
</script>

## Phase 0: FINM 32500 Homeworks

Github repo: [`./finm-python`](https://github.com/dafu-zhu/finm-python)

* [x] HW 1 (Session 1-2): CSV-Based Algorithmic Trading Backtester
* [x] HW 2 (Session 3): Multi-Signal Strategy Simulation on S&P 500
* [x] HW 3 (Session 4): Runtime & Space Complexity in Financial Signal Processing
* [x] HW 5 (Session 5-6): Testing & CI in Financial Engineering
* [x] HW 6 (Session 7): Design Patterns in Financial Software Architecture
* [ ] HW 7 (Session 8): Parallel Computing for Financial Data Processing
* [ ] HW 8 (Session 9): Inter-Process Communication for Trading Systems
* [ ] HW 9 (Session 10): Mini Trading System

**Note:** HW 7-9 must be completed before starting Phase 1 of the trading system project.

## Objectives

### Primary Goals

1. **Course Requirement Fulfillment**
   * Complete all 4 parts of FINM 32500 final project specification
   * Implement production-quality components exceeding minimum requirements
   * Demonstrate mastery of course concepts (IPC, design patterns, testing, parallel computing)

2. **Institutional-Grade System Design**
   * Build event-driven architecture supporting both backtesting and live trading
   * Implement unified interface eliminating code divergence between modes
   * Design for scalability and maintainability beyond course scope

3. **Resume/Portfolio Enhancement**
   * Showcase skills directly applicable to quant developer roles
   * Demonstrate proficiency with broker APIs, real-time data processing, and deployment
   * Evidence production-quality code with testing, documentation, and monitoring

### Success Metrics

* **Functionality**: Pass all project requirements (Parts 1-4)
* **Architecture**: Event-driven design with <50ms tick-to-signal latency
* **Reliability**: Zero position discrepancies during Alpaca paper trading
* **Quality**: >80% test coverage with comprehensive documentation
* **Performance**: Process 1000+ ticks/second with multi-threaded architecture

## Definition of Done

### Part 1: Data Download and Preparation ✅

**Step 1: Data Acquisition**
* Download intraday data using yfinance (equity) or Binance API (crypto)
* Store as CSV: Datetime, Open, High, Low, Close, Volume
* Support multiple symbols and timeframes (1min, 5min, 1hour)

**Step 2: Data Cleaning**
* Remove duplicates and NaN values
* Set Datetime as index, ensure chronological ordering
* Add derived features: returns, moving averages, volatility indicators
* Store cleaned data in Parquet format for fast access

**Step 3: Trading Strategy Class**
* Implement as Python class with clear interface:
  * `generate_signals(data)`: Returns buy/sell signals
  * `calculate_position_size(signal, capital)`: Determines order quantity
  * `get_stop_loss()`, `get_take_profit()`: Risk management levels
* Support multiple strategies (MA crossover, momentum, mean reversion)
* Strategy parameters configurable via constructor

### Part 2: Backtester Framework ✅

**Step 1: Gateway for Data Ingestion**
* Gateway class streaming CSV data row-by-row
* Support both historical (backtest) and live (Alpaca) data sources
* Emit MarketEvent for each new tick/bar
* Handle missing data and market hours gracefully

**Step 2: Order Book Implementation**
* OrderBook class using heaps for bid/ask management
* Price-time priority matching logic
* Support operations: add, modify, cancel, match orders
* Track order state transitions with timestamps
* O(log n) insertion, O(1) best bid/ask retrieval

**Step 3: Order Manager & Gateway**
* OrderManager validation checks:
  * Capital sufficiency (cash available ≥ order value)
  * Rate limiting (orders/minute threshold)
  * Position limits (max long/short exposure)
* Gateway audit logging:
  * Log all order events (sent, modified, cancelled, filled)
  * Write to CSV/database with timestamps
  * Support replay for debugging

**Step 4: Matching Engine Simulator**
* MatchingEngine class simulating execution:
  * Randomly determine fill/partial fill/cancel outcomes
  * Configurable fill probability and slippage model
  * Return execution details (price, quantity, timestamp, fees)
* Support order types: market, limit, stop-loss

### Part 3: Strategy Backtesting ✅

**Simulation Execution:**
* Feed historical data through Gateway → Strategy → OrderManager → MatchingEngine
* Process signals in chronological order (no look-ahead bias)
* Maintain portfolio state (positions, cash, unrealized P&L)

**Performance Tracking:**
* Record all executed trades with full details
* Calculate metrics:
  * P&L (realized, unrealized, total)
  * Sharpe ratio, Sortino ratio, Calmar ratio
  * Maximum drawdown, average drawdown duration
  * Win/loss ratio, profit factor
  * Fill rate, average slippage

**Reporting:**
* Equity curve visualization (matplotlib/plotly)
* Trade distribution histograms
* Performance statistics table
* Parameter sensitivity analysis (heat maps)

### Part 4: Alpaca Trading Challenge ✅

**Step 1-3: Account Setup**
* Create Alpaca paper trading account
* Obtain and secure API keys (environment variables)
* Test authentication with basic API calls

**Step 4: Market Data Retrieval**
* Use Alpaca SDK for data fetching
* Support both historical bars and real-time streaming
* Handle rate limits and connection errors

**Step 5: Data Storage**
* Store in PostgreSQL for persistence (trades, orders, positions)
* Cache real-time state in Redis (current positions, pending orders)
* Archive historical data as Parquet files

**Step 6: Strategy Integration**
* Deploy Part 1 strategy with Alpaca live data
* Run in paper trading mode for validation
* Monitor performance and position reconciliation

### Additional Production Features

**Event-Driven Architecture:**
* Event bus using thread-safe queues
* Event types: MarketEvent, SignalEvent, OrderEvent, FillEvent
* Decoupled components communicating only via events

**Risk Management:**
* Pre-trade checks (position limits, drawdown circuit breakers)
* Real-time position reconciliation with broker
* Automated stop-loss order generation

**Testing & Quality:**
* Unit tests for all components (pytest, >80% coverage)
* Integration tests for order lifecycle
* End-to-end backtest validation against known results
* CI/CD pipeline (GitHub Actions)

**Deployment:**
* Docker containerization with docker-compose
* Environment-based configuration (dev/paper/live)
* Logging infrastructure (structured logs, rotation)
* Monitoring dashboard (real-time P&L, system health)

## Technical Approaches

### 3.1 System Architecture

**Event-Driven Design:**

```
Data Gateway (CSV/Alpaca)
    ↓ MarketEvent
Event Bus (Queue)
    ↓
Strategy Engine
    ↓ SignalEvent
Event Bus
    ↓
Order Manager (Validation)
    ↓ OrderEvent
Event Bus
    ↓
Matching Engine / Alpaca Executor
    ↓ FillEvent
Event Bus
    ↓
Portfolio Tracker
```

**Key Principles:**
* Single Responsibility: Each component has one job
* Open/Closed: Easy to add new strategies without modifying core
* Dependency Inversion: Components depend on abstractions (Event types)

### 3.2 Order Book Implementation

**Data Structures:**
* Max heap for bids (highest price first)
* Min heap for asks (lowest price first)
* Hash map for O(1) order lookup by ID

**Operations:**
* `add_order(order)`: Insert into appropriate heap
* `cancel_order(order_id)`: Mark as cancelled, lazy deletion
* `match_orders()`: Match best bid/ask if price overlaps
* `get_best_bid()`, `get_best_ask()`: O(1) peek at heap root

**Complexity Analysis:**
* Insert: O(log n)
* Cancel: O(log n) worst case (may need heap reorganization)
* Match: O(1) per match, O(k log n) for k matches

### 3.3 Gateway Abstraction

**Interface Design:**

```
DataGateway (Abstract)
    ├── CSVGateway (Backtesting)
    │   └── stream_historical_data()
    └── AlpacaGateway (Live)
        └── stream_live_data()

ExecutionGateway (Abstract)
    ├── SimulatedExecutor (Backtesting)
    │   └── simulate_execution()
    └── AlpacaExecutor (Live)
        └── submit_order()
```

**Benefits:**
* Strategy code unchanged between backtest and live
* Easy to add new data sources (IEX, Polygon, etc.)
* Testable with mock gateways

### 3.4 Strategy Framework

**Base Strategy Class Interface:**
* `on_market_data(event)`: Process new market data, return signals
* `calculate_position_size(signal)`: Determine order quantity
* `get_parameters()`: Return strategy configuration
* `get_risk_limits()`: Define stop-loss, take-profit levels

**Concrete Strategies (Minimum 3):**

1. **MA Crossover**: Fast MA crosses slow MA → signal
2. **Momentum**: RSI-based entry/exit rules
3. **Mean Reversion**: Bollinger Band touches → contrarian trades

### 3.5 Risk Management

**Pre-Trade Validation:**

| Check | Criteria | Action if Failed |
|-------|----------|------------------|
| Capital Sufficiency | cash ≥ order_value + buffer | Reject order |
| Rate Limit | orders_sent < max_per_minute | Queue order |
| Position Limit | abs(position) ≤ max_position | Reduce size or reject |
| Drawdown Circuit Breaker | current_dd < max_drawdown | Halt all trading |

**Post-Trade Monitoring:**
* Position reconciliation every 60 seconds (compare system vs Alpaca)
* Alert on discrepancies >$0.01 or >0.001 shares
* Daily end-of-day reconciliation report

### 3.6 Performance Analytics

**Backtest Metrics:**

* **Risk-Adjusted Returns:**
  * Sharpe Ratio = (Return - RiskFree) / Volatility
  * Sortino Ratio = (Return - RiskFree) / Downside Deviation
  * Calmar Ratio = Annual Return / Max Drawdown

* **Drawdown Analysis:**
  * Maximum drawdown (peak-to-trough)
  * Drawdown duration (time underwater)
  * Recovery time

* **Trade Statistics:**
  * Win rate = Winning Trades / Total Trades
  * Profit factor = Gross Profit / Gross Loss
  * Average win/loss ratio

**Live Monitoring:**
* Real-time P&L (mark-to-market)
* Execution quality (slippage, fill rate)
* System health (latency, message throughput)

### 3.7 Data Pipeline

**Historical Data:**
* Source: yfinance for equities (7 days, 1min bars)
* Storage: Parquet files (compressed, columnar)
* Loading: Polars for fast I/O (5-10x faster than pandas)

**Live Data:**
* Source: Alpaca Data API v2 (WebSocket streaming)
* Protocol: Async WebSocket with automatic reconnection
* Buffering: Redis for tick cache (last 1000 ticks per symbol)

**Data Quality:**
* Validate OHLC consistency (O,H,L,C ordering, H≥L)
* Remove duplicate timestamps
* Interpolate missing bars (forward-fill)

### 3.8 Concurrency Model

**Threading Strategy:**
* **Main thread**: Event loop processing events from queue
* **Data thread**: WebSocket listener, feeds MarketEvents
* **Execution thread**: Order submission to Alpaca API
* **Monitoring thread**: Dashboard updates, reconciliation

**Thread Safety:**
* Use thread-safe queue for event bus
* Lock portfolio state updates (position modifications)
* Atomic operations for order ID generation

**Async I/O:**
* Alpaca WebSocket using asyncio
* Database writes using async PostgreSQL driver
* HTTP requests using async HTTP client

### 3.9 Testing Strategy

**Unit Tests (pytest):**
* Event creation and serialization
* Order book operations (add, cancel, match)
* Strategy signal generation (deterministic inputs)
* Risk check logic (capital, limits)

**Integration Tests:**
* End-to-end order flow: Signal → Validation → Execution → Fill
* Database persistence and retrieval
* Gateway switching (CSV → Alpaca)

**System Tests:**
* Backtest MA crossover strategy, compare to known benchmark
* Paper trading for 24 hours, verify zero discrepancies
* Stress test: 10,000 ticks/second data feed

**Mocking:**
* Mock Alpaca API responses
* Simulated market data generator
* Deterministic random seeds for reproducible tests

### 3.10 Technology Stack

**Language:** Python 3.11+

**Core Libraries:**
* **Data**: Polars (DataFrames), NumPy, yfinance
* **Events**: queue (thread-safe), asyncio
* **Database**: PostgreSQL, Redis
* **Broker**: alpaca-trade-api, websockets
* **Testing**: pytest, pytest-asyncio, pytest-cov
* **Visualization**: plotly, matplotlib, dash
* **Deployment**: Docker, docker-compose

**Infrastructure:**
* **Database**: PostgreSQL 15 (trades, orders, positions)
* **Cache**: Redis 7 (real-time state)
* **Deployment**: Docker on AWS EC2 (optional) or local

## Development Timeline

### Phase 0: FINM 32500 Completion (Before Nov 23)

**Prerequisites:**
* Complete HW 7: Parallel Computing for Financial Data Processing
* Complete HW 8: Inter-Process Communication for Trading Systems
* Complete HW 9: Mini Trading System

**Deliverable**: All homework implementations in `finm-python` repo

### Phase 1: Foundation (Week 1: Nov 23-29)

**Part 1 & Part 2 Core:**
* Data download and cleaning (yfinance)
* Strategy class implementation (3 strategies)
* Gateway class (CSV streaming)
* Order Book implementation (heaps)
* **Deliverable**: Backtest MA crossover on historical data

### Phase 2: Backtesting System (Week 2: Nov 30 - Dec 6)

**Part 2 & 3 Completion:**
* Order Manager with validation logic
* Matching Engine simulator
* Event bus architecture
* Portfolio tracker with P&L calculation
* Performance analytics module
* **Deliverable**: Complete backtester with 3 strategies tested

### Phase 3: Live Trading Integration (Week 3: Dec 7-13)

**Part 4 Implementation:**
* Alpaca account setup and API testing
* Alpaca Gateway (WebSocket + REST)
* Alpaca Executor implementation
* Position reconciliation logic
* **Deliverable**: Paper trading system running for 3 days

### Phase 4: Production Features (Week 4: Dec 14-20)

**Polish & Deploy:**
* Docker containerization
* PostgreSQL/Redis integration
* Monitoring dashboard (Dash)
* Comprehensive test suite (>80% coverage)
* CI/CD pipeline (GitHub Actions)
* Documentation (README, architecture diagrams)
* **Deliverable**: Production-ready system with full documentation

### Phase 5: Buffer & Optimization (Week 5: Dec 21-24)

**Final Polish:**
* Performance optimization
* Bug fixes from paper trading observations
* Additional test cases
* Final report with backtesting results
* **Deliverable**: Submit project by Dec 24, 11:59pm

## Risk Mitigation

| Risk | Mitigation |
|------|------------|
| Alpaca API rate limits | Implement exponential backoff, cache data locally |
| Thread synchronization bugs | Use proven thread-safe primitives, extensive testing |
| Position discrepancies | Automated reconciliation, alerts on mismatch >$0.01 |
| Data quality issues | Validation layer, handle missing/invalid data |
| Scope creep beyond course | Prioritize Part 1-4 completion first, add features after |
| Time constraints | 5-week timeline with 1-week buffer, can defer AWS deployment |
| Strategy performance | Focus on system quality, not alpha generation |
| Homework delays | Start Phase 1 only after HW 7-9 complete |

## Success Indicators

**Quantitative:**
* Pass all project requirements (Parts 1-4): 100/100 points
* Process 1000+ ticks/second with multi-threading
* Zero position reconciliation errors over 3-day paper trading
* >80% test coverage across all components
* Backtest 3+ strategies with full performance reports

**Qualitative:**
* Clean, modular codebase following SOLID principles
* Production-grade deployment with Docker
* Comprehensive documentation suitable for handoff
* Resume-worthy achievement demonstrating systems engineering skills

## Deliverables Checklist

### Course Requirements
* Part 1: CSV data files + cleaned DataFrames + 3 strategy classes
* Part 2: Gateway, OrderBook, OrderManager, MatchingEngine classes
* Part 3: Backtesting results with performance metrics and visualizations
* Part 4: Alpaca integration with paper trading execution logs

### Production Additions
* `/src` with modular components (events, strategies, execution, risk, portfolio)
* `/tests` with pytest suite (>80% coverage)
* `/data` with historical data and schemas
* `/docs` with architecture diagrams and API documentation
* `README.md` with setup, usage, design decisions
* `docker-compose.yml` for full system deployment
* `.github/workflows/ci.yml` for automated testing
* Performance comparison report (backtest vs paper trading)

## Learning Outcomes from FINM 32500 Integration

**Homework Foundations Applied:**

* **HW 1-2**: CSV backtesting → Generalized to event-driven backtester
* **HW 3-4**: Complexity analysis → Optimized data structures in order book
* **HW 5**: Testing/CI → Comprehensive test suite with GitHub Actions
* **HW 6**: Design patterns → Observer pattern for event bus, Strategy pattern for algos
* **HW 7**: Parallel computing → Multi-threaded data processing
* **HW 8**: IPC → Thread-safe queue for event communication
* **HW 9**: Mini system → Foundation for full production system

## Future Enhancements (Post-Project)

**Advanced Features:**
* Machine learning signal integration (pre-trained models)
* Multi-asset support (futures, options)
* Advanced order types (TWAP, VWAP, iceberg)
* GPU acceleration for backtesting
* Web-based configuration interface

**Institutional Features:**
* FIX protocol support for alternative brokers
* Multi-venue execution with smart routing
* Market microstructure modeling (queue position, adverse selection)
* High-frequency optimization (C++ critical path)

---

<script>
window.addEventListener('load', function() {
  const projectStart = new Date(2025, 10, 25); // November 25, 2025 (month is 0-indexed)
  const today = new Date();
  
  const diffMs = today - projectStart;
  const diffDays = Math.floor(diffMs / (1000 * 60 * 60 * 24)) + 1; // +1 to count start day as Day 1
  
  if (diffDays < 1) {
    document.getElementById('progress').textContent = 'Not started yet';
  } else {
    const weekNum = Math.ceil(diffDays / 7);
    const dayInWeek = ((diffDays - 1) % 7) + 1; // 1-7 instead of 0-6
    document.getElementById('progress').textContent = `Week ${weekNum}, Day ${dayInWeek}`;
  }
});
</script>
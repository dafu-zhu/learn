---
title: Alpha Miner
parent: Projects
nav_order: 2
---

# Auto Alpha Miner: Production-Grade System

**Project Start Date:** November 25, 2025  
**Progress:** <span id="progress">Loading...</span>

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

## Project Overview

Build an automated alpha factor mining system optimized for WorldQuant BRAIN platform submission, demonstrating production-quality infrastructure design, efficient factor generation, and rigorous validation pipelines.

## Objectives

### Primary Goals

1. **WorldQuant BRAIN Qualification**
   * Achieve Gold status (10,000+ points) to qualify for Research Consultant Program
   * Generate submittable alphas passing validation tests
   * Target quarterly consultant compensation ($2,000+ for Master level)

2. **Production System Development**
   * Build scalable infrastructure processing 500+ alpha candidates per hour
   * Implement 8-stage validation pipeline with 10-15% pass rate
   * Deploy as containerized microservice (Docker + AWS)

3. **Resume/Portfolio Enhancement**
   * Demonstrate systems engineering skills (not just ML research)
   * Showcase production deployment experience
   * Build portfolio-worthy automated system

### Success Metrics

* **Performance**: 500+ alphas/hour generation throughput
* **Quality**: 10-15% validation pass rate (50-75 valid alphas/hour)
* **Deployment**: Live FastAPI service with <100ms API latency
* **Documentation**: Production-grade README, API docs, architecture diagrams

## Definition of Done

### A. Functional System

* Generate 500+ alpha expressions per hour
* Pass all 8 validation stages before manual submission
* Achieve 10%+ conversion rate through validation pipeline
* Export alphas in WorldQuant-compatible format for manual submission
* Successfully submit alphas meeting minimum thresholds:
  * Sharpe > 1.25
  * 1% < Turnover < 70%
  * Fitness ≥ 1.0
  * IS (In-Sample) validity confirmed

### B. System Architecture

* Modular Python codebase (type hints, docstrings, tests)
* FastAPI REST API with endpoints:
  * `POST /generate` - Start generation run
  * `GET /alphas/{id}` - Retrieve alpha details
  * `POST /validate` - Validate specific expression
  * `GET /metrics` - System performance dashboard
  * `GET /export` - Export validated alphas for manual submission
* Docker containerization (multi-stage build)
* AWS deployment (EC2 or Lambda + S3)
* Redis caching for operator results (optional but recommended)
* **API abstraction layer ready for future WorldQuant BRAIN API integration**

### C. Documentation

* Comprehensive README with:
  * Architecture overview diagram
  * Setup/deployment instructions
  * Performance benchmarks
* API documentation (OpenAPI/Swagger)
* Code comments and docstrings
* Design decision log

### D. Testing & Quality

* Unit tests for critical components (pytest, >70% coverage)
* Integration tests for validation pipeline
* Performance profiling reports
* CI/CD pipeline (GitHub Actions)

## Technical Approaches

### 3.1 Alpha Generation Engine

**Primary: Reinforcement Learning (PPO/REINFORCE)**

* State: Current formula AST representation
* Action: Select next token (operator/operand)
* Reward: Weighted combination of:
  * Sharpe ratio (primary)
  * Fitness score
  * Diversity penalty (correlation with existing pool)
  * Validity bonus (passes syntax checks)
* Tech Stack: PyTorch, custom AST parser

**Baseline: Genetic Programming**

* Tree-based GP with crossover/mutation
* Simple baseline for comparison
* Custom GP engine with WorldQuant operator grammar

### 3.2 WorldQuant Operator Grammar

**Operator Categories:**

1. **Arithmetic**: `+, -, *, /, ^, abs, log, sign`
2. **Time Series**: `ts_sum, ts_mean, ts_std_dev, ts_rank, ts_max, ts_min, ts_decay_linear, ts_arg_max, ts_arg_min`
3. **Cross-Sectional**: `rank, scale, group_rank, group_scale`
4. **Vector**: `max, min, correlation, covariance`
5. **Logical**: `if_else, <, >, ==`
6. **Special**: `delay, delta, signedpower`

**Data Fields**: `open, high, low, close, volume, vwap, returns, adv{N}`

**Grammar Constraints:**

* Maximum depth: 7-10 levels
* Type system: Ensure float/vector compatibility
* Lookback limits: ts_* operators typically 5-252 days
* NaN handling: Operators must handle missing data gracefully

### 3.3 Validation Pipeline (8-Stage)

| Stage | Check | Reject If |
|-------|-------|-----------|
| 1. Syntax | Parse against WorldQuant grammar | Parsing fails |
| 2. Type Checking | Verify data type compatibility | Type mismatch detected |
| 3. Complexity Filter | Check formula depth ≤ 10 | Exceeds complexity budget |
| 4. Execution Test | Run on sample dataset (100 stocks × 500 days) | Runtime > 5s OR crashes |
| 5. NaN/Inf Filter | Calculate % of valid values | >5% NaN/Inf in output |
| 6. Correlation Check | Compute correlation with existing pool | max(abs(correlation)) > 0.7 |
| 7. Performance Threshold | Local backtest simulation (2016-2021) | Sharpe < 1.25 OR Turnover ∉ [1%, 70%] OR Fitness < 1.0 |
| 8. Export Validation | Format check for WorldQuant platform | Invalid expression format |

**Expected Pass Rates:**

* Stage 1-4: ~70% (syntax/runtime filters)
* Stage 5-6: ~30% (quality filters)
* Stage 7: ~15% (performance threshold)
* Stage 8: ~12-15% (final export-ready)

**Note:** Stage 7 uses local historical data simulation. After consultant status, Stage 8 will be replaced with actual WorldQuant BRAIN API submission.

### 3.4 Backtesting Engine

**Architecture:**

```
Data Layer (Polars DataFrames)
    ↓
Operator Cache (Redis/File)
    ↓
Expression Evaluator (Vectorized NumPy)
    ↓
Metrics Calculator (Sharpe, Turnover, Fitness)
    ↓
Validation Pipeline
```

**Key Features:**

1. **Vectorization**: Use Polars lazy evaluation + NumPy operations
   * Process 3000 stocks × 1260 days in <1 second
   
2. **Caching**: Store intermediate results
   * Redis for distributed, file-based for local development
   
3. **Parallelization**: Python multiprocessing
   * Worker pool (8-16 processes) evaluates alphas in parallel
   * Shared memory for price data

**Performance Target**: <10ms per alpha evaluation

**Data Requirements:**

* Historical price data (OHLCV) for US equities (TOP3000 universe)
* 5+ years of daily data (2016-2021 minimum)
* Sources: Yahoo Finance, Alpha Vantage, or similar free APIs
* Stored in Parquet format for fast access

### 3.5 API Abstraction Layer

**Design Pattern**: Strategy pattern with pluggable backends

**Current Implementation (Manual Export):**

* Export validated alphas to JSON/CSV format
* Include expression + expected metrics (Sharpe, Turnover, Fitness)
* Batch export functionality for efficient manual submission
* Copy-paste ready format for WorldQuant BRAIN platform

**Future Implementation (Post-Consultant):**

* Same interface, different backend
* Drop-in WorldQuant BRAIN API client
* No changes to core generation/validation logic
* Automatic submission with rate limiting and retry logic

**Interface Design:**

```
AlphaSubmitter (Abstract)
    ├── ManualExporter (Current)
    │   └── export_to_file()
    └── BRAINAPIClient (Future)
        └── submit_to_platform()
```

### 3.6 Deployment Architecture

**Container Structure:**

* Multi-stage Docker build
* Separate services: API, Worker Pool, Cache
* Docker Compose for local development
* Production deployment on AWS

**AWS Setup:**

* **Compute**: EC2 t3.medium (2 vCPU, 4GB RAM) ~$30/month
* **Storage**: S3 for alpha history, model checkpoints, exported alphas
* **Cache**: ElastiCache Redis (t3.micro) ~$15/month
* **Monitoring**: CloudWatch logs + custom Dash dashboard

**Alternative**: Docker Compose with Redis container (local-first)

### 3.7 Alpha Diversity Mechanisms

1. **Reward Shaping**: 
   * Balance Sharpe, Fitness, and diversity in reward function
   * Penalize correlation with existing alpha pool

2. **Experience Replay with Diversity Filter**:
   * Store alphas in replay buffer
   * Sample training batches with low intra-batch correlation
   
3. **Exploration Bonuses**:
   * Entropy regularization in policy
   * Novelty bonus for unused operator combinations

### 3.8 Export Format

**JSON Output Structure:**

```
{
  "alpha_id": "alpha_001",
  "expression": "rank(ts_sum(close, 20) / ts_std_dev(close, 20))",
  "metrics": {
    "sharpe": 1.45,
    "turnover": 0.23,
    "fitness": 1.12,
    "returns": 0.087,
    "drawdown": 0.065
  },
  "settings": {
    "region": "USA",
    "universe": "TOP3000",
    "delay": 1,
    "neutralization": "MARKET",
    "truncation": 0.08
  },
  "validation_status": "PASSED",
  "timestamp": "2025-01-15T10:30:00Z"
}
```

**Batch Export:**

* CSV format with one alpha per row
* Copy-paste directly into WorldQuant BRAIN interface
* Automatic deduplication against previously submitted alphas

## Development Timeline

### Phase 1 (Week 1): Core Engine

* RL agent (basic PPO) with WorldQuant grammar
* Simple backtester (Polars + NumPy) using free historical data
* Stage 1-5 validation pipeline
* **Deliverable**: Generate 100 valid alphas locally

### Phase 2 (Week 2): Validation & Export

* Stage 6-8 validation (correlation + performance + export)
* Export functionality (JSON/CSV formats)
* Batch processing capabilities
* **Deliverable**: Export 50+ validated alphas ready for manual submission

### Phase 3 (Week 3): Production Features

* FastAPI REST endpoints
* Multi-processing parallel evaluation
* Redis caching (optional)
* Docker containerization
* **Deliverable**: 500 alphas/hour throughput

### Phase 4 (Week 4): Polish & Deploy

* AWS deployment (EC2 + S3)
* Documentation (README, API docs)
* Performance benchmarking report
* CI/CD pipeline (GitHub Actions)
* **Deliverable**: Live system + comprehensive docs

## Tools & Technologies

**Language**: 100% Python 3.11+

**Core Libraries:**

* **Data**: Polars, NumPy, PyArrow
* **RL**: PyTorch
* **API**: FastAPI, httpx, pydantic
* **Deployment**: Docker, boto3, uvicorn
* **Testing**: pytest, hypothesis, locust
* **Caching**: Redis (optional), diskcache

**Data Sources:**

* Yahoo Finance API (yfinance)
* Alpha Vantage
* Polygon.io (free tier)

**Future Integration:**

* WorldQuant BRAIN API (post-consultant status)

## Risk Mitigation

| Risk | Mitigation |
|------|------------|
| RL doesn't converge | Start with GP baseline, iterate RL design |
| Data quality issues | Multiple data sources, validation checks |
| Low pass rate (<5%) | Tune reward function, add pre-filters |
| Deployment complexity | Start local, defer AWS until proven |
| Feature creep | MVP first (RL + validation + export), iterate |
| Time constraints | 4-week timeline with weekly checkpoints |
| Manual submission overhead | Batch export, clipboard-ready format |

## Success Indicators

**Quantitative:**

* 500+ alphas/hour generation rate
* 10-15% end-to-end pass rate
* Export 50+ validated alphas per day
* Reach Gold status (10,000 points) within 6-8 weeks of manual submission
* API p99 latency <100ms

**Qualitative:**

* Clean, readable codebase (type hints, >70% test coverage)
* Production-grade deployment (Docker + cloud)
* Professional documentation
* Resume-worthy: "Built automated alpha mining system deployed on AWS, processing 500+ factors/hour with 12% validation pass rate"

## Deliverables Checklist

* GitHub repository with clean commit history
* `README.md` with architecture diagram and setup instructions
* `requirements.txt` / `pyproject.toml`
* `Dockerfile` and `docker-compose.yml`
* `/app` directory with modular codebase
* `/tests` directory with pytest suite (>70% coverage)
* `/docs` with API documentation and design decisions
* CI/CD configuration (`.github/workflows/`)
* Performance benchmarking report
* `/exports` directory with sample validated alphas
* Data pipeline documentation for historical data ingestion

## WorldQuant Submission Requirements

**Hard Requirements:**

* Sharpe > 1.25
* 1% < Turnover < 70%
* Fitness ≥ 1.0
* IS_valid = True (validated after manual submission)

**Soft Guidelines:**

* Higher Sharpe (>2.0 excellent)
* Lower turnover (15-30% ideal balance)
* Market neutralization recommended
* Low correlation with existing portfolio

**Common Rejection Reasons:**

* Too many NaN values (>5%)
* Excessive complexity (depth >10)
* High correlation with existing alphas
* Violates data look-ahead bias rules

## Future Enhancements (Post-Consultant)

**Phase 5: API Integration**

* WorldQuant BRAIN API client implementation
* Automatic submission with rate limiting
* Real-time performance tracking
* Portfolio correlation monitoring

**Phase 6: Advanced Features**

* Multi-objective optimization (Pareto front)
* Ensemble alpha generation
* Adaptive learning from submission feedback
* GPU acceleration for backtesting


## Appendix

A book to RL: [Reinforcement Learning: An Overview](/assets/pdfs/projects/rl_overview.pdf)

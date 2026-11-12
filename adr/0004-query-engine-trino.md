# ADR 0004: Interactive query engine: Trino

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** FR-EXP-001, FR-EXP-004, FR-PRC-001, FR-SHR-006, NFR-PRF-001, NFR-PRF-003, NFR-SCL-003

## Context

Analysts write SQL and dashboards run queries over Iceberg tables. Targets: p95 < 5 s on 10M rows (P1) rising to < 3 s on 1B rows (P3), many workspaces on shared compute, and row-level security and column masking in P2.

## Decision

Use **Trino** as the interactive SQL engine over the Iceberg catalog. Run one shared cluster in P1 with a **resource group per workspace** (concurrency, memory and CPU caps). Use Trino's **OPA access control** integration for table permissions, and in P2 for row filters and column masks. Cache dashboard results in **Redis**, keyed by query hash, workspace and table snapshot ID.

SQL transformations (FR-PRC-001) also run on Trino in P1, scheduled by Temporal.

## Consequences

- Positive: mature Iceberg support, ANSI SQL, scales horizontally.
- Positive: RLS and masking come from OPA rather than custom query rewriting.
- Positive: Apache 2.0 licensed; runs on-prem.
- Negative: JVM cluster with real memory needs; idle cost for the Free tier.
- Negative: not ideal for very large batch jobs; those move to Spark in P2 (ADR 0013).
- Follow-ups: P1 load test with 500 workspaces; define "10x faster" baseline (NFR-PRF-002).

## Alternatives considered

| Option | Why not |
| --- | --- |
| DuckDB per query | Excellent for small data and cheap, but single-node; can't meet 1B-row targets. Possible later as a fast path for tiny Free-tier queries. |
| Spark SQL only | Higher latency for interactive queries. |
| StarRocks / Doris | Fast, but smaller ecosystem and less mature Iceberg + OPA integration. |
| ClickHouse | See ADR 0003. |

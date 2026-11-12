# ADR 0003: Lakehouse storage: Apache Iceberg on object storage

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** FR-INT-004, FR-PRC-003, FR-EXP-007, NFR-SCL-001, NFR-DAT-001, NFR-AVL-002

## Context

We need to store customer data from 15 → 200+ connectors, query it interactively, process billions of rows in P3, and keep it from 7 days (Free) to custom retention (Enterprise). Several engines will touch the same data: Trino (P1), Flink and Spark (P2).

## Decision

All ingested and transformed data is stored as **Apache Iceberg** tables (Parquet files) on S3 / S3-compatible object storage. Each workspace gets its own Iceberg namespace and storage prefix. Table metadata is served by an **Iceberg REST catalog**; the implementation (Apache Polaris or Lakekeeper) is chosen by a P1 spike.

## Consequences

- Positive: schema evolution supports automatic schema detection and changes (FR-INT-004).
- Positive: open format read by Trino, Spark and Flink; engines can change without data migration.
- Positive: snapshots give table rollback (RPO help) and snapshot expiry implements retention.
- Positive: object storage is the cheapest option for long retention.
- Negative: needs maintenance jobs (compaction, snapshot expiry, orphan file cleanup) run by Temporal.
- Negative: sub-second latency for streaming data is not achievable on Iceberg alone (see ADR 0012).
- Follow-ups: catalog spike; define partitioning defaults per connector type.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Delta Lake | Strong in Spark ecosystem; Iceberg has broader multi-engine and REST catalog support. |
| ClickHouse as primary store | Fast queries, but storage tied to one engine; harder Spark/ML access; on-prem scaling burden. |
| Snowflake / BigQuery as backend | No on-prem path; cost passes through to Free tier. |
| PostgreSQL for customer data | Does not scale to billions of rows. |

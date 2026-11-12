# ADR 0012: Streaming ingest: Kafka + Flink into Iceberg

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P2
- **Requirements:** FR-INT-005, FR-INT-006, NFR-SCL-002

## Context

P2 adds real-time sources (Kafka, Kinesis, webhooks) and change data capture (CDC), with a source-to-queryable target of < 60 s. P3 tightens that to < 5 s ("real-time" on the website).

## Decision

- **P2:** Run **Apache Kafka** (via the Strimzi operator on Kubernetes) as the ingest buffer, and **Apache Flink** jobs written in **Java** (Flink 2.0 removed the Scala API) that write to Iceberg with commits every ≤ 60 s. CDC uses Debezium into Kafka. Webhooks are received by the API and published to Kafka.
- **P3 (< 5 s):** **Open.** Iceberg commit frequency can't reliably go that low. Candidates: a hot serving store beside Iceberg (e.g. ClickHouse or Apache Pinot) queried through Trino, or Trino's Kafka connector over recent data. Decide during P2 with a spike.

## Consequences

- Positive: standard open-source streaming stack; runs on-prem.
- Negative: Kafka and Flink are heavy to operate; only Business tier pays for them.
- Negative: small-file buildup from frequent commits needs compaction jobs.
- Follow-ups: P3 real-time serving ADR; per-workspace topic quotas.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Amazon MSK / Kinesis only | No on-prem path; allowed later as a SaaS implementation of the same interface. |
| Redpanda | Kafka-compatible and simpler, but source-available license (BSL); review before bundling. |
| Spark Structured Streaming | Workable; Flink gives lower latency and better CDC support. |

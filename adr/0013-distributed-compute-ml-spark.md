# ADR 0013: Distributed compute and ML: Spark on Kubernetes

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P2
- **Requirements:** FR-PRC-003, FR-PRC-005, FR-PRC-006, NFR-SCL-001

## Context

P2 needs 1B-record batch jobs and an ML pipeline (classification, regression, clustering: train, evaluate, score). P3 targets billions of records. Trino is not built for long, heavy batch jobs or model training.

## Decision

Run **Apache Spark** on Kubernetes (Spark Operator), reading and writing Iceberg, launched as Temporal activities. Platform-owned heavy jobs (compaction, large ETL) are written in **Scala**; ML uses **PySpark + MLlib** for the three model families (ADR 0011). Model artifacts and metrics are stored in object storage, registered in PostgreSQL, and scored results are written back to Iceberg tables for dashboards (FR-PRC-006).

Small datasets may train with scikit-learn in a Python worker for speed; the API hides which runtime runs a job.

## Consequences

- Positive: one engine for large ETL and ML; Iceberg-native.
- Positive: autoscaling executors per job (NFR-SCL-004).
- Negative: Spark start-up time adds latency for small jobs.
- Negative: MLlib covers classic ML only; deep learning is out of scope.
- Follow-ups: decide model registry approach (e.g. MLflow) during P2.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Ray | Strong for Python ML, weaker for SQL-style ETL; two engines to run. |
| Trino for everything | Not suited to long batch and training workloads. |
| Managed EMR / Databricks | No on-prem path; cost pass-through. |

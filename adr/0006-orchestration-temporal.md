# ADR 0006: Job orchestration: Temporal

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** FR-INT-002, FR-INT-009, FR-PRC-002, FR-PRC-004

## Context

We run scheduled syncs, transforms and maintenance jobs for thousands of workspaces. In P2, users build their own visual workflows (DAGs with schedules, dependencies, retries). Jobs must survive restarts, retry correctly, and report status to the UI.

## Decision

Use **Temporal** (MIT) for all job orchestration. Workflow definitions are written in Go next to the Platform API; activities run in the language of their component (Python connectors and ML, Scala Spark jobs, Java Flink jobs) on separate task queues. Every sync, transform, maintenance and user workflow is a Temporal workflow, tagged with `workspace_id`. The P2 visual workflow builder stores a DAG definition in PostgreSQL; a generic Temporal workflow interprets it at runtime.

## Consequences

- Positive: durable execution, retries, timeouts and schedules built in.
- Positive: workflows are created dynamically from API calls; fits multi-tenant, user-defined DAGs better than DAG-file tools.
- Positive: self-hostable for on-prem.
- Negative: Temporal cluster is another stateful service to operate (uses PostgreSQL in our setup).
- Negative: no built-in data-lineage UI; lineage (FR-EXP-007) is built by us.
- Follow-ups: per-workspace concurrency limits via task queues.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Apache Airflow | DAGs as code files; awkward for thousands of tenants creating DAGs through a UI. |
| Dagster | Strong for a single data team's assets; less suited as a multi-tenant job engine behind a product. |
| Kubernetes CronJobs + queue | We would rebuild retries, state and history ourselves. |

# Architecture Decision Records

Each ADR records one decision: the context, what we chose, what we gave up, and the alternatives. Format: [Michael Nygard's ADR template](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions). See [ADR 0001](0001-record-architecture-decisions.md).

**Status values:** Proposed → Accepted → (Deprecated | Superseded by ADR-nnnn). Every ADR below starts as **Proposed** until the team reviews it. Never edit an accepted ADR's decision; write a new ADR that supersedes it.

| ADR | Title | Status | Phase |
| --- | --- | --- | --- |
| [0001](0001-record-architecture-decisions.md) | Record architecture decisions | Proposed | P1 |
| [0002](0002-kubernetes-everywhere.md) | Kubernetes everywhere, AWS first | Proposed | P1 |
| [0003](0003-lakehouse-iceberg.md) | Lakehouse storage: Apache Iceberg on object storage | Proposed | P1 |
| [0004](0004-query-engine-trino.md) | Interactive query engine: Trino | Proposed | P1 |
| [0005](0005-connector-framework-dlt.md) | Connector framework: dlt-based workers | Proposed | P1 |
| [0006](0006-orchestration-temporal.md) | Job orchestration: Temporal | Proposed | P1 |
| [0007](0007-postgresql-metadata-store.md) | Metadata store: PostgreSQL | Proposed | P1 |
| [0008](0008-multi-tenancy-model.md) | Multi-tenancy model: shared by default, dedicated by tier | Proposed | P1 |
| [0009](0009-identity-keycloak.md) | Identity with Keycloak, authorization with OPA | Proposed | P1 |
| [0010](0010-secrets-management.md) | Secrets: AWS Secrets Manager on SaaS, OpenBao on-prem | Proposed | P1 |
| [0011](0011-application-stack.md) | Languages per component: Go, Python, Scala, Java, TypeScript | Proposed | P1 |
| [0012](0012-streaming-kafka-flink.md) | Streaming ingest: Kafka + Flink into Iceberg | Proposed | P2 |
| [0013](0013-distributed-compute-ml-spark.md) | Distributed compute and ML: Spark on Kubernetes | Proposed | P2 |
| [0014](0014-usage-metering.md) | Usage metering and billing | Proposed | P1 |
| [0015](0015-observability.md) | Observability: OpenTelemetry stack | Proposed | P1 |

New ADR: copy [template.md](template.md), take the next number, add a row here.

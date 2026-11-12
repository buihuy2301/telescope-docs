# ADR 0011: Languages per component

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** FR-EXP-001..004, FR-PRC-001..006, FR-INT-001..009, FR-SHR-002/005/007

## Context

The team codes Go, Java and other languages, and wants the right language for each component rather than one language everywhere. The data ecosystem forces some choices: dlt and ML libraries are Python, Trino plugins are Java, Flink removed its Scala API in Flink 2.0. Components talk through APIs, Temporal task queues and Iceberg tables, not shared code, so polyglot is practical.

## Decision

| Component | Language | Key libraries / tools | Why |
| --- | --- | --- | --- |
| Platform API, gateway logic, metering, billing integration | **Go** | net/http + chi or Echo, oapi-codegen, pgx, sqlc, Atlas or golang-migrate | Fast, small containers, strong concurrency; OPA embeds as a Go library. |
| Temporal workflow definitions (orchestration logic) | **Go** | Temporal Go SDK | Lives next to the API; workflows call activities in other languages via task queues. |
| Connector workers | **Python** | dlt, Temporal Python SDK | dlt is Python-only; largest connector ecosystem. |
| ML and data science (FR-PRC-005/006, AI insights later) | **Python** | PySpark + MLlib, scikit-learn, pandas / Polars | Where the ML ecosystem and data scientists are. |
| Platform-owned Spark jobs: Iceberg compaction, snapshot expiry, large ETL (FR-PRC-003) | **Scala** | Spark Scala API, sbt | Typed Datasets and UDFs without Python serialization cost, for heavy jobs we own. Java is an acceptable substitute. |
| Flink streaming jobs (P2) | **Java** | Flink DataStream / Table API, Iceberg Flink sink | Flink 2.0 dropped the Scala API. |
| Trino plugins (access control, event listener) | **Java** | Trino SPI | Required by Trino. |
| Web app | **TypeScript** | React, Monaco editor, Apache ECharts | Standard frontend stack; ECharts covers 50+ chart types, embedding and theming. |
| Infrastructure | HCL / YAML | OpenTofu, Helm | See ADR 0002. |

### Boundary rules

1. **One writer for metadata.** Only the Go Platform API writes PostgreSQL. Python, Scala and Java jobs report results through Temporal activity results or the internal API, never by writing tables directly.
2. **Contracts are generated, not hand-copied.**
   - REST: OpenAPI spec is the source of truth → Go server stubs (oapi-codegen) and TypeScript client.
   - Job inputs/outputs and usage events: Protobuf or JSON Schema in a shared `contracts/` folder → generated types for Go, Python, Java/Scala.
3. **Data contracts are Iceberg tables.** Jobs in any language exchange data only through Iceberg tables in the workspace namespace.
4. **Language cap.** New languages need a new ADR. Current set: Go, Python, Scala, Java, TypeScript.

### Repository layout (monorepo)

```
contracts/      OpenAPI, Protobuf / JSON Schema
api/            Go: Platform API, Temporal workflows, metering
web/            TypeScript: React app
workers/
  connectors/   Python: dlt-based connector activities
  ml/           Python: ML training and scoring
  spark/        Scala: platform Spark jobs
  flink/        Java: streaming jobs (P2)
  trino-plugins/ Java: Trino SPI plugins
deploy/         Helm charts, OpenTofu
```

## Consequences

- Positive: each component uses the language its ecosystem is strongest in.
- Positive: Go API is cheap to run and simple to deploy; OPA can be embedded for low-latency authorization checks.
- Negative: five languages means five build toolchains in CI (Go, uv/pip, sbt, Gradle/Maven, pnpm) and broader code-review skills.
- Negative: domain models exist in several languages; mitigated by generated contracts.
- Follow-ups: CI templates per language; code owners per folder; decide sbt vs. Gradle for Scala.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Python everywhere except frontend | Fewer languages, but the team prefers Go for services, and Trino/Flink still need Java. |
| Scala for Flink | Flink 2.0 removed the Scala API. |
| Scala for all Spark work, including ML | ML libraries and data scientists are in Python; PySpark DataFrame jobs run at the same speed through Spark's optimizer. |
| Kotlin instead of Go for the API | Fine on the JVM, but Go gives smaller containers and the team already writes Go. |

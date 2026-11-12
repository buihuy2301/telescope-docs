# ADR 0005: Connector framework: dlt-based workers

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** FR-INT-001..005, FR-INT-007..009

## Context

The website promises 200+ connectors (15 in P1, 60 in P2). Writing and maintaining them all ourselves is the largest scope risk. We need schema detection, incremental sync, light transforms, and output to Iceberg. Components must be legally redistributable for on-prem (P3) and usable in a hosted SaaS.

## Decision

Build connector workers in **Python on [dlt](https://dlthub.com/) (data load tool, Apache 2.0)**. dlt provides schema inference and evolution, incremental loading, normalization, and verified sources for common databases and SaaS APIs. Workers run as Kubernetes jobs started by Temporal and write Parquet / Iceberg.

We define a thin **Telescope connector interface** (config schema, check, discover, read) around dlt, so that:

- we can wrap other sources (e.g. MIT-licensed Singer taps) where dlt lacks coverage;
- the P3 customer-facing connector SDK (FR-INT-008) is this same interface.

## Consequences

- Positive: library, not a platform; embeds cleanly in our worker and multi-tenant model.
- Positive: Python is the data ecosystem language; workers talk to the Go control plane only through Temporal and generated contracts (ADR 0011).
- Negative: dlt's source catalog is smaller than the largest connector ecosystems; gaps need custom code.
- Negative: we own connector quality, testing and certification.
- Follow-ups: spike to map the P1 top 15 connectors to dlt sources; certification test suite per connector.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Airbyte platform | Largest catalog, but the platform is under the Elastic License v2, which restricts offering it as a managed service; connector licenses vary. Needs legal review before any use. |
| Fivetran / managed ELT | No on-prem; per-row cost breaks Free tier economics. |
| Singer taps only | Uneven quality and maintenance; used selectively behind our interface. |
| Build everything | Not feasible for 200+ connectors. |

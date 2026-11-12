# ADR 0007: Metadata store: PostgreSQL

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** FR-PLT-001..004, FR-SHR-001, NFR-SEC-003, NFR-AVL-002

## Context

The control plane stores workspaces, users, roles, sources, job history, saved queries, dashboards, shares and usage counters. This data is relational, transactional and must be isolated per tenant.

## Decision

Use **PostgreSQL** as the single metadata store, written only by the Platform API. Every tenant-owned table has a `workspace_id` column, and **PostgreSQL row-level security** policies enforce it as a second line of defense behind application checks. SaaS uses Amazon RDS (multi-AZ, point-in-time recovery); on-prem uses CloudNativePG or a customer database.

## Consequences

- Positive: well-known, transactional, works everywhere.
- Positive: database-level tenant enforcement catches application bugs.
- Negative: usage events at high volume may outgrow it; revisit in P2 (ADR 0014).
- Follow-ups: schema migrations owned by the Go API (Atlas or golang-migrate); RLS policy tests in CI.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Separate database per tenant | Too much overhead for thousands of Free workspaces; kept as a P3 option. |
| MySQL | No advantage for us; weaker RLS support. |
| Document store | Data is relational; transactions matter. |

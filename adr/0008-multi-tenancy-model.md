# ADR 0008: Multi-tenancy model: shared by default, dedicated by tier

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** NFR-SEC-003, NFR-SCL-003, NFR-SEC-001, FR-PLT-003

## Context

The Free tier must be cheap to run at 500 (P1) to 5,000 (P2) workspaces. Business customers expect stronger isolation and no noisy neighbors. Enterprise expects full isolation and data sovereignty.

## Decision

The tenant unit is the **workspace**. Isolation grows by tier:

| Layer | Free (P1) | Business (P2) | Enterprise (P3) |
| --- | --- | --- | --- |
| Metadata | Shared PostgreSQL + RLS | Same | Dedicated |
| Storage | Shared bucket, prefix + Iceberg namespace per workspace | + per-workspace KMS key | Customer storage |
| Query | Shared Trino, resource group per workspace | Optional dedicated Trino | Dedicated |
| Jobs | Shared pools, concurrency caps | Optional dedicated pools | Dedicated |

Rules from P1:

- Every API request resolves one `workspace_id` from the session; it is never taken from client input alone.
- Storage access is via scoped credentials per workspace prefix, not one global key.
- Cross-tenant access tests (API, Trino, storage) run in CI.

## Consequences

- Positive: low cost per Free workspace; clear upsell path to isolation.
- Positive: the same code path serves shared and dedicated, only configuration differs.
- Negative: shared tiers depend on correct resource limits; needs load testing.
- Follow-ups: define per-plan resource group sizes; noisy-neighbor alerting.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Fully dedicated stack per tenant | Unaffordable for Free tier. |
| Fully shared for all tiers | Fails Enterprise isolation and sovereignty. |

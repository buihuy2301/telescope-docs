# ADR 0014: Usage metering and billing

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1 (metering), P2 (billing)
- **Requirements:** FR-PLT-003, FR-PLT-004, FR-PLT-005, NFR-DAT-001

## Context

Plans limit data sources (5 on Free), processing (1 GB/month Free, 100 GB/month Business) and retention (7 days, 1 year, custom). The definition of "processing GB" is still an open question (see [pricing](../04-pricing-and-plan-limits.md#open-questions)). Business is billed per user per month with a 14-day trial.

## Decision

- Every worker and query emits **usage events** (`workspace_id`, kind, bytes ingested, bytes scanned, compute seconds, timestamp). All three measures are recorded from P1, so the billing metric can be picked later without new instrumentation.
- Events are written to an append-only PostgreSQL table, rolled up hourly per workspace. Revisit (e.g. ClickHouse or Iceberg table) if volume exceeds what PostgreSQL handles comfortably.
- The Platform API enforces limits **before** starting a job or query (soft warning at 80%, block at 100% for Free).
- **Billing (P2):** **Stripe** for subscriptions, seats and trials. On-prem uses a signed license key instead.

## Consequences

- Positive: limits and usage dashboard (FR-PLT-004) come from the same data.
- Positive: the metering metric can change without code changes in workers.
- Negative: enforcement adds a check to every job and query start.
- Follow-ups: decide the billing metric and overage policy before the P1 launch.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Metering from cloud bills | Not per-workspace; not available on-prem. |
| Third-party metering service | Extra vendor and cost for a small P1 need. |

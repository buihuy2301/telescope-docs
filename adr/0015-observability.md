# ADR 0015: Observability: OpenTelemetry stack

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** NFR-OPS-001, NFR-AVL-001, NFR-PRF-001..003, NFR-CMP-001

## Context

We must prove uptime (99.5% → 99.9%), latency targets and sync health, alert before customers notice, and keep logs for SOC 2. On-prem customers need the same tooling without our cloud accounts.

## Decision

Instrument all services with **OpenTelemetry**. Metrics in **Prometheus**, logs in **Loki**, traces in **Tempo** (P2), dashboards and alerts in **Grafana**. Every signal carries `workspace_id` where it applies. Define SLOs for API availability, query p95 and sync success rate from P1; a public status page in P2.

## Consequences

- Positive: vendor-neutral and bundleable for on-prem.
- Positive: per-workspace signals support support tickets and noisy-neighbor detection.
- Negative: we operate the stack ourselves; storage costs for logs grow with tenants.
- Follow-ups: log retention policy aligned with SOC 2; PII scrubbing in logs.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Datadog / New Relic | Fast to start, costly at scale, not usable on-prem. Could be added on SaaS via OpenTelemetry export. |
| CloudWatch only | AWS-only. |

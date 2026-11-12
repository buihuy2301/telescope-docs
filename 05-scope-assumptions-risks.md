# Scope, Assumptions, Risks and Open Questions

As of 2026-11-12. See [README](README.md) for phase and ID conventions.

## Out of scope for P1

Streaming, ML, public API, embedding, RLS/masking, SSO, on-prem, billing beyond the trial, white-label.

## Assumptions

- We build on open-source parts where possible (connector framework, query engine, orchestrator) and don't write all 200+ connectors ourselves.
- SaaS runs on one cloud provider in P1. Which provider and region is still open.
- On-prem uses the same containers as SaaS (Kubernetes), so P3 doesn't fork the codebase.

## Risks

| Risk | Impact | Mitigation |
| --- | --- | --- |
| Website promises features before they ship | Customer trust, legal exposure (SOC 2, SLA claims) | Mark tiers and claims by phase on the site |
| 200+ connectors is too much to maintain | Broken syncs, support load | Reuse an open-source connector ecosystem; certify a core set |
| On-prem doubles the release and support work | P3 slips | Design for it from P1 (containers, no SaaS-only services in the core) |
| "Processing GB" metering is vague | Billing disputes, wrong cost model | Define the metric before P1 build (see [pricing questions](04-pricing-and-plan-limits.md#open-questions)) |
| AI "automatic insights" quality | Misleading results | Ship as beta in P2 with feedback loop |

## Open questions

- [ ] Target first customer segment and the top 15 connectors for P1
- [ ] Team size and target date for each phase exit
- [ ] Cloud provider and first region
- [ ] Baseline for the "10x faster" claim
- [ ] Build vs. buy for the query engine and connectors

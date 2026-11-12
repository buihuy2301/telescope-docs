# Telescope Data Platform — Docs

As of 2026-11-12.

The website ([nauxs.com](https://nauxs.com/)) describes the target product. These docs keep every number on it and assign each one to a delivery phase. Phase 1 ships a usable Free tier, Phase 2 the Business tier, and Phase 3 reaches the full website promise with Enterprise and on-prem.

## Documents

| # | File | Contents |
| --- | --- | --- |
| 1 | [01-product-brief.md](01-product-brief.md) | Vision, customers, personas, phase overview and exit gates |
| 2 | [02-functional-requirements.md](02-functional-requirements.md) | Functional requirements by module, with phase per requirement |
| 3 | [03-non-functional-requirements.md](03-non-functional-requirements.md) | Non-functional requirements with numeric targets per phase |
| 4 | [04-pricing-and-plan-limits.md](04-pricing-and-plan-limits.md) | Pricing tiers and plan limits mapped to phases |
| 5 | [05-scope-assumptions-risks.md](05-scope-assumptions-risks.md) | Out of scope, assumptions, risks and open questions |
| 6 | [06-architecture.md](06-architecture.md) | Architecture overview: principles, components, flows, tenancy, deployment, NFR mapping |
| 7 | [07-data-model.md](07-data-model.md) | Data model / ERD: PostgreSQL metadata schema and Iceberg data layout |
| 8 | [adr/](adr/README.md) | Architecture Decision Records (ADR 0001–0015) |

## Conventions

- **Phases:** P1 = MVP (Free tier, SaaS), P2 = Business tier (SaaS), P3 = Enterprise and target state (SaaS + on-prem).
- **Requirement IDs:** `FR-<module>-<nnn>` for functional and `NFR-<area>-<nnn>` for non-functional requirements. Modules: INT (integration), PRC (processing), EXP (exploration), SHR (sharing), PLT (platform, admin, billing).
- **Priority within a phase:** Must / Should / Could. A Must that slips blocks the phase exit.
- Architecture docs, tickets and tests refer back to these IDs. Change a target here first, then downstream.

## Next docs

Once these docs are agreed, write the following, in this order. Only steps 1–3 must be finished before P1 coding starts.

1. ~~**Architecture overview and ADRs**~~ — drafted: [06-architecture.md](06-architecture.md), [adr/](adr/README.md). Review and accept the ADRs.
2. ~~**Data model / ERD**~~ — drafted: [07-data-model.md](07-data-model.md). Confirm its assumptions A1–A7.
3. **P1 module PRDs:** user stories with acceptance criteria for INT, PRC, EXP, SHR, PLT (P1 rows only).
4. **API spec (OpenAPI):** internal API in P1, public REST API for P2.
5. **Test and benchmark plan:** how each NFR target is measured (load tests, failover drills, the 10x benchmark).
6. **Security and compliance plan:** SOC 2 control list and evidence collection from P1.
7. **Delivery roadmap:** milestones and team assignment per phase, after estimation.

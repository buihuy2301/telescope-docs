# Pricing Tiers and Plan Limits

As of 2026-11-12. See [README](README.md) for phase and ID conventions.

Each tier goes live in the phase that can deliver all of its listed features. Until then, the website should show it as "coming soon" or as a waitlist.

| Tier | Price (website) | Live from | Limits and features (website) |
| --- | --- | --- | --- |
| Free (SaaS) | $0, forever | P1 | Up to 5 data sources, 1 GB processing/month, basic dashboards, standard SQL, community support, 7-day retention |
| Business (SaaS) | $99 per user/month, 14-day trial without credit card | P2 | Unlimited sources, 100 GB processing/month, advanced dashboards, ML models, real-time streaming, priority support, 1-year retention, API access |
| Enterprise (on-prem) | Custom | P3 | Data sovereignty, unlimited processing, custom integrations, advanced security, dedicated support, custom retention, SLA, white-label |

All plans: SSL encryption and regular backups (NFR-SEC-001, NFR-AVL-002).

## Open questions

- What does "1 GB processing" measure: data ingested, data scanned by queries, or compute time? Metering (FR-PLT-003) can't be built until this is defined.
- What happens when a workspace hits a limit: hard stop, throttle, or overage charge?
- Is the 100 GB on Business per workspace or per user?

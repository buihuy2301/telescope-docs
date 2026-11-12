# Non-Functional Requirements

As of 2026-11-12. See [README](README.md) for phase and ID conventions.

The website numbers (99.9% uptime, billions of records, 10x faster, real-time, SOC 2, GDPR) are the P3 column. P1 and P2 targets are proposed steps toward them and need engineering sign-off. Every target must be measurable in a load test or monitoring dashboard.

| ID | Area | P1 (MVP) | P2 (Business) | P3 (Target, website) |
| --- | --- | --- | --- | --- |
| NFR-AVL-001 | Monthly uptime, SaaS | 99.5%, no SLA | 99.9% SLA (about 43 min downtime/month) | 99.9% SLA on SaaS, contractual SLA on Enterprise |
| NFR-AVL-002 | Backups, recovery (RPO / RTO) | Daily backups; RPO 24 h, RTO 8 h | RPO 1 h, RTO 4 h | RPO 15 min, RTO 1 h |
| NFR-PRF-001 | Interactive SQL query, p95 | < 5 s on 10M rows | < 3 s on 100M rows | < 3 s on 1B rows |
| NFR-PRF-002 | "10x faster" claim | Define baseline and benchmark | Publish internal benchmark | Public benchmark vs. named baseline |
| NFR-PRF-003 | Dashboard load, p95 | < 3 s | < 2 s | < 2 s |
| NFR-SCL-001 | Batch processing volume per job | 10M records | 1B records | Billions of records in seconds (define dataset) |
| NFR-SCL-002 | Streaming latency, source to queryable | Not in P1 | < 60 s | < 5 s ("real-time") |
| NFR-SCL-003 | Concurrent workspaces | 500 | 5,000 | Thousands of companies |
| NFR-SCL-004 | Auto-scaling | Manual scaling | Auto-scale compute | Auto-scale compute and storage |
| NFR-SEC-001 | Encryption | TLS 1.2+ in transit, AES-256 at rest | + customer data keys per tenant | End-to-end; customer-managed keys (BYOK) |
| NFR-SEC-002 | Connector credentials | Stored in a secrets manager, never logged | Same + rotation | Same + customer vault integration |
| NFR-SEC-003 | Tenant isolation | Logical isolation, tested | + isolated compute option | Full isolation; on-prem data sovereignty |
| NFR-SEC-004 | Access control | RBAC | + RLS, column masking, audit | + SSO, SCIM, advanced controls |
| NFR-CMP-001 | SOC 2 | Controls designed and evidence collected | SOC 2 Type I | SOC 2 Type II |
| NFR-CMP-002 | GDPR | Privacy policy, data deletion on request, EU-compliant subprocessors | DPA, data residency choice (EU/US) | Full compliance program |
| NFR-DAT-001 | Data retention by plan | 7 days (Free) | 1 year (Business) | Custom (Enterprise) |
| NFR-OPS-001 | Observability | Logs, metrics, alerts on syncs and queries | + tracing, status page | + customer-facing health views |
| NFR-OPS-002 | Support response | Community | Priority (define hours) | Dedicated, 24/7 |
| NFR-DEP-001 | Deployment | Single-region SaaS | Multi-region SaaS | SaaS + on-prem (Kubernetes) |

SOC 2 Type II needs an audit window of several months, so evidence collection must start in P1, even though the claim only goes live in P3.

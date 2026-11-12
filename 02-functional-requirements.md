# Functional Requirements

As of 2026-11-12. See [README](README.md) for phase and ID conventions.

Each website feature is listed once, with the phase it first ships in. Numbers that grow over time (connectors, chart types) show the target per phase. Each row is a starting point; detailed user stories and acceptance criteria come in the per-module PRDs.

## Data Integration (INT)

| ID | Requirement | P1 | P2 | P3 | Priority |
| --- | --- | --- | --- | --- | --- |
| FR-INT-001 | Pre-built connectors: SQL and NoSQL databases, cloud storage, files, APIs, SaaS apps | 15 | 60 | 200+ | Must |
| FR-INT-002 | Batch ingestion with schedule (hourly, daily, cron) and manual run | Yes | Yes | Yes | Must |
| FR-INT-003 | File upload (CSV, JSON, Parquet, Excel) | Yes | Yes | Yes | Must |
| FR-INT-004 | Automatic schema detection and schema-change handling | Detect | Detect + evolve | Same | Must |
| FR-INT-005 | Incremental sync (cursor or CDC) instead of full reload | Cursor | + CDC | Same | Should (P1) |
| FR-INT-006 | Real-time streaming sources (Kafka, Kinesis, webhooks) | No | Yes | Yes | Must (P2) |
| FR-INT-007 | Transformation during ingestion (rename, cast, filter, dedupe) | Basic | Full ETL | Same | Should |
| FR-INT-008 | Connector SDK for custom integrations | No | Internal | Customer-facing | Must (P3) |
| FR-INT-009 | Sync monitoring: status, row counts, errors, retries, alerts | Yes | Yes | Yes | Must |

## Data Processing (PRC)

| ID | Requirement | P1 | P2 | P3 | Priority |
| --- | --- | --- | --- | --- | --- |
| FR-PRC-001 | SQL transformations saved as reusable models | Yes | Yes | Yes | Must |
| FR-PRC-002 | Visual workflow orchestration: DAG of steps, schedules, dependencies, retries | No | Yes | Yes | Must (P2) |
| FR-PRC-003 | Distributed compute for large jobs (billions of records) | No | Partial | Yes | Must (P3) |
| FR-PRC-004 | Auto-scaling of compute based on load | No | Yes | Yes | Must (P2) |
| FR-PRC-005 | ML pipeline: classification, regression, clustering; train, evaluate, score | No | Yes | Yes | Must (P2) |
| FR-PRC-006 | Predictive modeling outputs usable in dashboards | No | Yes | Yes | Should |

## Data Exploration (EXP)

| ID | Requirement | P1 | P2 | P3 | Priority |
| --- | --- | --- | --- | --- | --- |
| FR-EXP-001 | SQL query editor with autocomplete, history, saved queries | Yes | Yes | Yes | Must |
| FR-EXP-002 | Drag-and-drop dashboard builder | Basic | Advanced | Same | Must |
| FR-EXP-003 | Visualization types | 10 | 30 | 50+ | Must |
| FR-EXP-004 | Dashboard filters, drill-down, scheduled refresh | Filters | + drill-down | Same | Must |
| FR-EXP-005 | Statistical functions and hypothesis testing in SQL | Basic stats | + tests | Same | Should |
| FR-EXP-006 | Smart Filtering: AI-powered data discovery and automatic insight generation | No | Beta | GA | Must (P3) |
| FR-EXP-007 | Data catalog: browse tables, columns, descriptions, lineage | Browse | + lineage | Same | Should |

## Data Sharing (SHR)

| ID | Requirement | P1 | P2 | P3 | Priority |
| --- | --- | --- | --- | --- | --- |
| FR-SHR-001 | Share dashboards and queries with users and teams, with view/edit permissions | Yes | Yes | Yes | Must |
| FR-SHR-002 | Export charts and query results (CSV, PNG, PDF) | CSV | All | All | Must |
| FR-SHR-003 | REST API for data and metadata, with API keys and rate limiting | No | Yes | Yes | Must (P2) |
| FR-SHR-004 | Public (unauthenticated) dashboard links | No | Yes | Yes | Should |
| FR-SHR-005 | Embed charts and dashboards in external apps | No | Yes | Yes | Must (P2) |
| FR-SHR-006 | Row-level security and column masking | No | Yes | Yes | Must (P2) |
| FR-SHR-007 | White-label: custom logo, domain, colors | No | No | Yes | Must (P3) |

## Platform, admin and billing (PLT)

| ID | Requirement | P1 | P2 | P3 | Priority |
| --- | --- | --- | --- | --- | --- |
| FR-PLT-001 | Sign-up, email login, workspace (tenant) creation | Yes | Yes | Yes | Must |
| FR-PLT-002 | Role-based access: Admin, Editor, Viewer | Yes | + custom roles | Same | Must |
| FR-PLT-003 | Plan limits enforced: sources, processing GB/month, retention | Yes | Yes | Yes | Must |
| FR-PLT-004 | Usage metering dashboard per workspace | Basic | Full | Same | Must |
| FR-PLT-005 | Billing: per-user subscription, 14-day trial without credit card | Trial only | Yes | + invoicing | Must (P2) |
| FR-PLT-006 | SSO (SAML/OIDC) and SCIM user provisioning | No | SSO | + SCIM | Must (P3) |
| FR-PLT-007 | Audit log of user and admin actions | No | Yes | + export | Must (P2) |
| FR-PLT-008 | On-premises installer and upgrade path | No | No | Yes | Must (P3) |
| FR-PLT-009 | Support channels: community, priority, dedicated | Community | Priority | Dedicated | Must |

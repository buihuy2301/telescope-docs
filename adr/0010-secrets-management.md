# ADR 0010: Secrets: AWS Secrets Manager on SaaS, OpenBao on-prem

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** NFR-SEC-001, NFR-SEC-002, NFR-CMP-001

## Context

Connectors hold customer database passwords, API tokens and keys. A leak would be the worst incident the platform can have. Credentials must never sit in PostgreSQL or logs, and P3 customers may require their own vault.

## Decision

Introduce a **secrets interface** in the Platform API (`put`, `get`, `rotate`, `delete`, scoped by workspace and source). Implementations:

- SaaS: **AWS Secrets Manager**, encrypted with KMS.
- On-prem: **OpenBao** (MPL 2.0 fork of HashiCorp Vault), or the customer's vault via the same interface (P3).

PostgreSQL stores only the secret reference. Connector workers fetch credentials at runtime using short-lived, workspace-scoped identities and never write them to logs.

## Consequences

- Positive: credentials isolated from metadata; supports rotation (P2) and customer vaults (P3).
- Negative: two implementations to test.
- Follow-ups: log scrubbing in the worker framework; secret access is audited.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Encrypted column in PostgreSQL | Simpler, but one database compromise exposes all credentials. |
| HashiCorp Vault everywhere | BSL license since 2023; OpenBao covers the on-prem case. |

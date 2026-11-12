# ADR 0002: Kubernetes everywhere, AWS first

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** NFR-DEP-001, FR-PLT-008, NFR-SCL-004, NFR-AVL-001

## Context

SaaS ships first (P1, P2), but P3 promises on-premises deployment with data sovereignty. If the SaaS core depends on cloud-only managed services (e.g. a serverless query service), on-prem becomes a second product. We also need autoscaling (P2) and a 99.9% SLA.

## Decision

All Telescope services run as containers on Kubernetes, packaged as Helm charts. SaaS runs on **AWS EKS**, one region in P1. Infrastructure is defined in **OpenTofu**; deployments use GitOps (Argo CD).

Managed AWS services are allowed only behind an interface that has an on-prem equivalent:

| Need | SaaS | On-prem equivalent |
| --- | --- | --- |
| Object storage | S3 | S3-compatible storage |
| PostgreSQL | RDS | CloudNativePG or customer DB |
| Secrets | Secrets Manager + KMS | OpenBao |

## Consequences

- Positive: one deployment artifact for SaaS and on-prem; portable to other clouds later.
- Positive: Kubernetes autoscaling covers NFR-SCL-004.
- Negative: Kubernetes operations skill is required from day one.
- Negative: we give up some convenient AWS-only services (e.g. Athena, Glue).
- Follow-ups: on-prem install testing from P2 on a non-AWS cluster.

## Alternatives considered

| Option | Why not |
| --- | --- |
| AWS-native serverless (Lambda, Athena, Glue) | Fast for P1, but no on-prem path; P3 would be a rewrite. |
| GCP or Azure first | Equally valid; AWS chosen for the widest customer and hiring base. Revisit if a launch customer requires another cloud. |
| Terraform | Same model, but BSL license since 2023; OpenTofu is the MPL-licensed fork. |

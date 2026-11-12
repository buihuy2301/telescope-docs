# ADR 0009: Identity with Keycloak, authorization with OPA

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** FR-PLT-001, FR-PLT-002, FR-PLT-006, FR-PLT-007, FR-SHR-003, FR-SHR-006, NFR-SEC-004

## Context

P1 needs sign-up, email login and roles (Admin, Editor, Viewer). P2 adds custom roles, API keys, row-level security and column masking. P3 adds SAML/OIDC SSO and SCIM provisioning. All of it must run on-prem.

## Decision

- **Authentication:** **Keycloak** (Apache 2.0), one realm for SaaS with workspace membership in our API; per-customer realms or identity-provider brokering for SSO in P3.
- **Authorization:** **Open Policy Agent (OPA)** holds policies. The Platform API asks OPA for permission checks; Trino uses its OPA access-control plugin for table access and, from P2, row filters and column masks. Roles and grants are stored in PostgreSQL and pushed to OPA as data.
- **API keys (P2):** issued by the Platform API, hashed at rest, scoped to a workspace and role, and rate-limited at the gateway.

## Consequences

- Positive: one policy model for the UI, API and SQL engine.
- Positive: SSO and SCIM come from Keycloak and its extensions instead of being built.
- Negative: Keycloak and OPA are two more services to run and upgrade.
- Negative: OPA policies need their own tests.
- Follow-ups: audit log (P2) records every authorization decision that changes data or grants.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Auth0 / Okta / Cognito | Fast for SaaS, no on-prem path. |
| Build auth ourselves | Security risk; SSO/SCIM is a lot of work. |
| Apache Ranger | Strong for Hadoop-style stacks; heavier and less natural for our API-level checks. |

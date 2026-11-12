# ADR 0001: Record architecture decisions

- **Status:** Proposed
- **Date:** 2026-11-12
- **Phase:** P1
- **Requirements:** NFR-CMP-001

## Context

The platform will make many decisions with long-lived effects (storage format, engines, tenancy). New team members, auditors (SOC 2) and future us need to know why each choice was made.

## Decision

We record every significant architecture decision as an ADR in `docs/adr/`, one Markdown file per decision, numbered in sequence, using [template.md](template.md). A decision is significant if it is hard to reverse, affects more than one component, or changes an NFR outcome.

## Consequences

- Positive: decisions are reviewable in pull requests and traceable to requirement IDs.
- Positive: provides change-management evidence for SOC 2.
- Negative: small overhead per decision.
- Follow-ups: ADR review is part of the architecture review meeting.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Wiki pages | Drift from code; no review history. |
| No formal record | Reasons get lost; decisions get re-argued. |

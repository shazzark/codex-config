# Technical requirements document (TRD)

> **Purpose:** Define testable technical requirements, constraints, dependencies, and technical acceptance criteria. **Use when:** A Standard or Advanced project has meaningful integration, security/privacy, performance, reliability, platform, or operational constraints. For a small project, include a short technical-requirements section in a combined specification instead. **Related:** `prd.md` owns product behavior; `architecture.md` owns system structure and design decisions; `implementation-plan.md` sequences delivery.
>
> Mark each item `[Confirmed]`, `[Assumption]`, `[Recommendation]`, or `[Open question]`. Technical acceptance must name an observable verification method and agreed target where a threshold is needed. Keep status `Draft` until technical owners approve it.

**Status:** Draft | **Technical owner:** {{owner}} | **Version:** {{version}}

## Required sections

### Scope and links
{{Systems/features covered; link to the PRD and MVP scope.}}

### Technical requirements
| ID | Requirement | Rationale | Priority/status | Acceptance and verification |
|---|---|---|---|---|
| TR-01 | {{testable technical behavior or property}} | {{why needed}} | {{priority + certainty}} | {{test/inspection/measurement and agreed pass condition}} |

### Constraints
{{Required platforms, runtimes, protocols, policies, compatibility, accessibility, data-location, or team/environment constraints. Distinguish fixed constraints from preferences.}}

### Dependencies
{{Internal systems, external providers/tools, versions/contracts, access or permissions, owners, availability assumptions, and fallback behavior. Do not assume provider capabilities or approvals.}}

### Technical acceptance
{{Cross-cutting checks for security/privacy, performance, reliability/recovery, compatibility, accessibility, or operations that apply to this project. Link requirement IDs to planned evidence.}}

### Open technical decisions
{{Decision, owner, due point, impact if unresolved, and options to investigate.}}

## Optional sections

- Workload assumptions and measurement method.
- Threat/privacy constraints or data classification summary; detailed controls belong to security work.
- Deployment/runtime constraints and recovery expectations.
- Integration failure modes and service-level dependencies.
- Test environment and test-data constraints.

## Approval and change history

{{Approvers, date, version, changes, and remaining conditions. Put component diagrams in architecture.md and detailed provider flows in external-integrations.md.}}

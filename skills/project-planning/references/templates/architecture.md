# Architecture overview

> **Purpose:** Describe the major system parts, boundaries, dependencies, and approved design choices. **Use when:** More than one component/service/data boundary matters, or technical decisions need to be shared. **Related:** `trd.md` states technical requirements; `architecture-decision-record.md` records individual consequential decisions; `implementation-plan.md` phases delivery.
>
> Label facts, assumptions, recommendations, and open questions explicitly. Acceptance criteria should verify architectural constraints only where observable evidence is possible; avoid pretending a diagram proves quality. Keep status `Draft` until decision owners approve it.

**Status:** Draft | **Architecture owner:** {{owner}} | **Version:** {{version}}

## Required sections

### Context and drivers
{{Problem, important quality attributes, constraints, and links to brief/PRD/TRD.}}

### System boundary and actors
{{Users, external systems, trust/data boundaries, and what this system does not own.}}

### Major components and responsibilities
| Component | Responsibility/data owned | Calls or events | Failure/availability notes |
|---|---|---|---|
| {{component}} | {{purpose and owner}} | {{contract/neighbor}} | {{relevant behavior}} |

### Key flows
{{Request/event/data flow for the most important user journey. Use a diagram only if it adds clarity.}}

### Decisions and trade-offs
{{Chosen options and reasons; link to ADRs for decisions with long-term consequences. Include operational cost and alternatives.}}

### Evolution and compatibility
{{Relevant migration, API/schema compatibility, rollout, rollback/forward recovery, and unresolved changes.}}

## Optional sections

- Deployment/topology view and environment assumptions.
- Observability, capacity, availability, or recovery overview.
- Security/trust-boundary summary, with detailed controls owned by security work.
- Diagrams for context, component, sequence, or data flow.

## Validation and approval

{{Evidence or review needed for material constraints; approvers and date. Link TRD, data schema, integration specs, and implementation plan rather than copying them.}}

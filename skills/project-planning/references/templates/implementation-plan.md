# Implementation plan

> **Purpose:** Turn approved requirements into bounded delivery phases with verification and completion conditions. **Use when:** The user approves a project or feature plan. **Related:** `prd.md` and `trd.md` define needs; `architecture.md` describes boundaries; `tasks.md` decomposes an approved phase.
>
> Mark assumptions and unresolved dependencies; do not present estimates or targets as confirmed unless their owner approves them. Each phase needs observable acceptance criteria. Keep this plan `Draft` until scope and major decisions are approved; approval to plan is not approval to implement.

**Status:** Draft | **Plan owner:** {{owner}} | **Scope/version:** {{scope}}

## Required sections

### Approved inputs and open decisions
{{Links to approved or draft requirements; list open questions and blockers.}}

### Milestones and dependencies
{{Milestones, external dependencies, sequencing constraints, and decision gates. Avoid unsupported dates/estimates.}}

### Phases

#### Phase {{number}} - {{name}}
- **Objective:** {{outcome}}
- **Scope:** {{included work and explicit exclusions}}
- **Dependencies:** {{prior phases, people, providers, or decisions}}
- **Deliverables:** {{reviewable artifacts or behavior}}
- **Acceptance criteria:** {{testable completion conditions linked to requirements}}
- **Verification:** {{tests, reviews, or checks and environment}}
- **Risks/blockers:** {{known risk and mitigation/owner}}
- **Completion conditions:** {{evidence and approvals required to close phase}}

### Change process
{{How new requirements, changed assumptions, and scope trade-offs are recorded and re-approved.}}

## Optional sections

- Release sequencing, feature flags, migration/rollback, training, or launch monitoring.
- Resource/cost envelope when approved and supportable.

## Approval and handoff

{{Approver, approved scope/version, date, authorized starting phase, and explicit limits. Do not start coding until implementation is approved.}}

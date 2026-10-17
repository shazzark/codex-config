# MVP scope

> **Purpose:** Define the smallest release that delivers a useful outcome and can be evaluated. **Use when:** Prioritizing initial delivery or controlling a project's scope. **Related:** `project-brief.md` gives the why; `prd.md` defines selected behavior; `implementation-plan.md` sequences approved work.
>
> Mark items `[Confirmed]`, `[Assumption]`, `[Recommendation]`, or `[Open question]`; do not treat a proposed priority as approved. Acceptance criteria must be observable and trace to a user need. Keep status `Draft` until approved.

**Status:** Draft | **Owner:** {{decision owner}} | **Release/cut:** {{identifier}}

## Required sections

### MVP outcome
{{The user problem and outcome this release must address.}}

### Must-have capabilities
| Priority | Capability | User value | Acceptance signal | Dependencies/status |
|---|---|---|---|---|
| P0 | {{capability}} | {{value}} | {{observable result}} | {{dependency and certainty label}} |

### Explicit non-goals
{{Features, platforms, integrations, scale, or workflows intentionally excluded from this release.}}

### Later candidates
{{Potential enhancements, kept separate from committed scope.}}

### Scope constraints and open decisions
{{Risks, feasibility constraints, and decisions that block scope approval.}}

## Optional sections

- Release success measures and evaluation method.
- Cut line: capabilities to defer first if cost, schedule, or risk exceeds expectations.
- Migration or rollout requirements for an existing product.

## Approval

{{Approver, approved scope/version, date, and conditions. A planned MVP is not user-validated until users or stakeholders actually evaluate it.}}

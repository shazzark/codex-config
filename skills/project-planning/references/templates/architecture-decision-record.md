# Architecture decision record

> **Purpose:** Preserve the reasoning behind one consequential technical choice. **Use when:** A decision has lasting cross-module, operational, cost, security, or compatibility effects. **Related:** `architecture.md` owns the current system overview; `trd.md` defines constraints; `implementation-plan.md` tracks adoption. Do not create one for every routine choice.
>
> Separate confirmed context from assumptions and proposals. A decision is approved only by its owner. Acceptance and validation criteria should identify observable evidence that would confirm the decision or trigger its review. Keep this record linked from architecture.md.

**Status:** Proposed | Accepted | Superseded | **Decision owner:** {{owner}} | **Date:** {{date}}

## Required sections

### Context and drivers
{{Problem, constraints, requirement IDs, and evidence.}}

### Decision
{{Chosen option, certainty/approval, and scope.}}

### Options considered
| Option | Benefits | Costs/risks | Reason accepted/rejected |
|---|---|---|---|
| {{option}} | {{benefit}} | {{trade-off}} | {{reason}} |

### Consequences and validation
{{Expected effects, migration/rollback implications, observable validation, and signal that would trigger reconsideration.}}

## Optional sections

- Related security, data, availability, or cost analysis.
- Follow-up decisions or implementation tasks.

## Related documents and approval

{{Link architecture overview, TRD, risks, and implementation phase. Record approver(s), date, conditions, and superseded ADR if any.}}

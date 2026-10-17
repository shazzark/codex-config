# Risk register

> **Purpose:** Track uncertain events that could materially affect users, security, feasibility, delivery, or operations. **Use when:** A project has meaningful unknowns or consequences; keep it lightweight for low-risk work. **Related:** `project-brief.md` and `trd.md` provide context; `implementation-plan.md` schedules mitigations; ADRs explain decisions.
>
> Separate evidence from assumptions. Rate likelihood/impact using agreed scales, not false precision. Acceptance means the owner has accepted, reduced, transferred, or explicitly deferred the risk with a review trigger. Keep status `Draft` until owners review it.

**Status:** Draft | **Risk owner:** {{owner}} | **Review date:** {{date}}

## Required sections

| ID | Risk and cause | Impact | Likelihood/impact | Evidence or assumption | Mitigation/contingency | Owner and trigger/status |
|---|---|---|---|---|---|---|
| R-01 | {{event and why it could happen}} | {{user/project consequence}} | {{agreed scale}} | {{confirmed evidence or assumption}} | {{prevention and fallback}} | {{owner, signal, state}} |

## Optional sections

- Risk categories or scoring guide.
- Dependencies and linked requirements/decisions.
- Residual risk and accepted-by record.

## Related documents and approval

{{Link TRD, integration specs, ADRs, and implementation phases. Record review owner/date and risks explicitly accepted or still open.}}

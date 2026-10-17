# Testing and acceptance plan

> **Purpose:** Describe what evidence will show the approved product and technical requirements are met. **Use when:** Several boundaries, risks, release gates, or stakeholder sign-offs need coordination. **Related:** `prd.md` and `trd.md` own requirements; `implementation-plan.md` schedules verification. Use `$testing-engineering` for detailed test design and execution.
>
> Distinguish planned checks from executed evidence. Link each check to requirement IDs and use isolated data/systems. Acceptance criteria need an observable pass condition and an approver where judgment is required. Keep status `Draft` until stakeholders approve it.
>
> Label acceptance expectations as `[Confirmed]`, `[Assumption]`, `[Recommendation]`, or `[Open question]`; never report a planned check as a result.

**Status:** Draft | **Test owner:** {{owner}} | **Target release:** {{release}}

## Required sections

### Acceptance scope
{{Included features, platforms, users, and environments; link to MVP/PRD/TRD.}}

### Evidence matrix
| Requirement ID | Risk/behavior | Verification level/check | Test data/environment | Pass condition | Owner/status |
|---|---|---|---|---|---|
| {{ID}} | {{what can fail}} | {{unit/integration/contract/E2E/security/performance/manual}} | {{isolated target}} | {{observable result}} | {{owner, planned/not run/result}} |

### Release acceptance
{{Required approvals, blockers, known limitations, and release decision rule. Do not equate test coverage with correctness.}}

## Optional sections

- Accessibility, performance/load, recovery, privacy/security, compatibility, or device matrix.
- Rollout observation, monitoring, rollback trigger, and post-release checks.
- Test data creation/cleanup and environment ownership.

## Related documents and approval

{{Link requirements, risks, and implementation phase. Record approver, date, and checks still unexecuted.}}

# Application flow

> **Purpose:** Show how users move through important journeys and what they see in each state. **Use when:** Navigation, branching, permissions, failure states, or multi-step workflows need clarification. **Related:** `prd.md` owns business behavior; `design-system.md` owns shared interface rules; `architecture.md` covers system interactions.
>
> Label steps and rules `[Confirmed]`, `[Assumption]`, `[Recommendation]`, or `[Open question]`. Acceptance criteria should verify user-visible outcomes and important branches. Keep status `Draft` until product decisions are approved.

**Status:** Draft | **Product owner:** {{owner}} | **Platforms:** {{web/mobile/CLI/API or applicable clients}}

## Required sections

### Primary journey
**User and goal:** {{who is acting and what they need}}

1. {{Entry point and preconditions}}
2. {{User action and system response}}
3. {{Decision or alternate branch}}
4. {{Success state and next action}}

### Important alternate and failure states
| Trigger | User-visible result | Recovery/next step | Certainty/status |
|---|---|---|---|
| {{invalid input, no data, denied access, offline, provider failure, etc.}} | {{result}} | {{recovery}} | {{label}} |

### Acceptance checks
{{Observable checks for the core journey and high-risk branches; link to PRD requirement IDs.}}

## Optional sections

- Mermaid flow diagram when it is clearer than prose.
- Roles, navigation map, onboarding, notifications, or offline behavior.
- Loading, empty, error, retry, and accessibility considerations.

## Related documents and approval

{{Link PRD, design-system guide, and relevant technical documents. Record approver, date, and unresolved journey decisions.}}

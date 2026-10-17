# Data and database design

> **Purpose:** Capture the application's important data concepts, ownership, relationships, and integrity needs. **Use when:** Persistent or shared data materially shapes the product. **Related:** `prd.md` owns user-visible data rules; `trd.md` owns database-related constraints; `architecture.md` shows system boundaries. Use `$database-engineering` for detailed engine/schema design.
>
> Mark confirmed facts, assumptions, recommendations, and open questions. Acceptance criteria should verify integrity and access behavior; do not invent volumes or indexes. Keep status `Draft` until the data owner approves it.

**Status:** Draft | **Data owner:** {{owner}} | **Version:** {{version}}

## Required sections

### Data concepts and ownership
| Entity/record | Meaning | Authoritative owner | Sensitive/retention notes |
|---|---|---|---|
| {{concept}} | {{meaning}} | {{module/system}} | {{confirmed requirement or unknown}} |

### Relationships and lifecycle
{{Important relationships, creation/update/deletion lifecycle, and business invariants.}}

### Access patterns and integrity needs
{{Key reads/writes, constraints, uniqueness, consistency, transaction needs, and query patterns. Do not select indexes without access-pattern evidence.}}

### Acceptance and verification
{{How constraints, migrations, data ownership, retention, and important queries will be checked in an isolated environment.}}

## Optional sections

- Conceptual ERD or document shape.
- Candidate database and rationale, clearly marked as proposed until approved.
- Migration, seed/test-data, backup, deletion, or data-import considerations.

## Related documents and approval

{{Link PRD for business rules, TRD for technical constraints, and architecture for ownership boundaries. Record approver, date, assumptions, and unresolved decisions.}}

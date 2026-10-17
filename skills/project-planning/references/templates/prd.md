# Product requirements document (PRD)

> **Purpose:** Describe product behavior and business requirements. **Use when:** Multiple user journeys, roles, rules, or acceptance expectations need a shared source of truth. **Related:** `project-brief.md` explains context; `mvp-scope.md` defines the cut; `trd.md` owns technical constraints; `appflow.md` maps journeys.
>
> Label each requirement `[Confirmed]`, `[Assumption]`, `[Recommendation]`, or `[Open question]`. Acceptance criteria must be testable from an observable user or system outcome; do not make unapproved assumptions look final. Keep status `Draft` until approved.

**Status:** Draft | **Owner:** {{product decision owner}} | **Version:** {{version}}

## Required sections

### Problem and goals
{{Link to the project brief and name the outcomes this product change supports.}}

### Users and permissions
{{Relevant user groups and business-level access expectations. Detailed security design belongs in specialist work.}}

### In-scope behavior
For each capability:
- **Requirement:** {{observable behavior and business rule}}
- **Priority/status:** {{priority and certainty label}}
- **Acceptance criteria:** {{testable outcomes; use Given/When/Then if it clarifies context}}
- **Related journey/dependency:** {{appflow section, prerequisite, or external decision}}

### Out of scope
{{Explicitly excluded behavior and deferred capabilities.}}

### Product constraints and open questions
{{Policy, accessibility, localization, support, retention, or business-rule constraints that affect behavior. Separate unresolved decisions.}}

## Optional sections

- User stories or journey links, if they add context beyond the requirement.
- Analytics and success measures with event meaning and agreed targets.
- Content, notifications, roles, or lifecycle rules.
- Release, migration, or compatibility expectations.

## Approval and change history

{{Approver, date, version, material changes, and unresolved conditions. Link the TRD and implementation plan; do not copy their technical details here.}}

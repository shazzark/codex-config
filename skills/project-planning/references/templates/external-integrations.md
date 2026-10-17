# External integration

> **Purpose:** Define the contract and operational assumptions for one external provider, API, tool, or partner. **Use when:** The product depends on an external system or gives an AI agent tools. **Related:** `prd.md` describes user-visible behavior; `trd.md` states technical constraints; `architecture.md` shows the boundary. Use networking/security/database skills for detailed controls.
>
> Mark provider documentation and permissions as verified only after checking them. Label assumptions and open questions. Acceptance criteria must be testable in a sandbox/test environment or clearly identify evidence not yet available. Keep status `Draft` until owners approve.

**Status:** Draft | **Integration owner:** {{owner}} | **Provider/version:** {{known provider and API version}}

## Required sections

### Purpose and data/action boundary
{{User need, system responsibility, data sent/received, and permitted actions. For agent tools, state allowed operations and which actions require confirmation.}}

### Contract and access prerequisites
{{Endpoints/events, auth method, scopes/permissions, rate limits, quotas, API/version docs, account or contract requirements. Separate verified facts from unknown provider terms. Do not include secrets.}}

### Data mapping and lifecycle
{{Identifiers, fields, ownership, synchronization, retention/deletion, and consistency expectations.}}

### Failure and recovery behavior
{{Timeouts, retry/idempotency, rate limiting, provider outage, partial result, reconciliation, and user-visible fallback.}}

### Acceptance and verification
{{Sandbox contract checks, error/limit cases, authorization tests, and evidence needed before production use.}}

## Optional sections

- Webhook signature/verification and replay handling.
- Data processing/privacy agreement and regional constraints.
- Cost model, support/escalation, version sunset, and service-level commitments.
- Human review, tool confirmation, and audit history for AI actions.

## Related documents and approval

{{Link TRD, architecture, risk register, and PRD. Record provider access/permission owner, approvers, date, and unresolved contract questions.}}

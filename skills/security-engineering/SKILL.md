---
name: security-engineering
description: Design, implement, review, and verify application and platform security. Use when a task materially changes security controls, threat exposure, deployment configuration, or vulnerability response.
---

# Security Engineering

Apply language-independent security principles to frontend applications, APIs, databases, workers, containers, integrations, and CI/CD. Version 1 includes focused guidance for Node.js, TypeScript, React, Next.js, Express, Fastify, PostgreSQL, MongoDB, and Redis. Choose controls for the actual assets, threats, and operating environment; do not treat a checklist as proof of security.

This skill owns broader security reasoning and controls. Use [application and API security](references/application-api.md) for web/API risks, [runtime and delivery security](references/runtime-and-delivery.md) for framework, container, dependency, and CI/CD concerns, [data and integration security](references/data-and-integrations.md) for databases, caches, queues, and external services, and [verification and response](references/verification-and-response.md) for tests and vulnerability handling.

## Security workflow

1. Inspect the existing controls, relevant code paths, deployment configuration, dependencies, and tests before proposing changes. Preserve established protections and project conventions.
2. Identify important assets, users, trust boundaries, exposed entry points, attacker capabilities, likely failure paths, and business impact. Record assumptions and unknowns; prioritize realistic risks.
3. Choose controls proportionate to the risk and application. Prefer secure defaults, least privilege, defense in depth, and maintained libraries over custom security protocols or cryptography.
4. Implement controls at boundaries that every relevant path traverses. Enforce authorization and validation on the server, not only in the UI.
5. Add focused regression tests for the threat and negative path. Run only authorized checks against systems in scope; explain what a scan or test can and cannot establish.
6. Review logs, errors, configuration, secrets, rollout, and failure behavior. Report confirmed findings separately from hypotheses and recommendations, and state what was actually verified.

## Required safeguards

- Never hardcode, print, or expose credentials, private keys, tokens, or sensitive records. Use protected secret storage and redact logs.
- Do not invent cryptographic algorithms, authentication protocols, or security-sensitive parsers when established maintained options fit.
- Do not weaken an existing control without explicit authorization and a documented reason.
- Do not perform intrusive testing against a system without authorization and a defined scope. Do not make destructive or production security changes without approval.
- Treat input from users, files, databases, queues, and third-party services as untrusted until validated for the intended use.
- Apply least privilege and fail safely when a security decision or required control cannot be established.
- Test relevant denial, malformed-input, abuse, and failure cases; do not rely only on happy-path or mocked tests where a real boundary matters.
- Never say an application is completely secure. Distinguish implemented, tested, untested, and recommended protections.
- Explain important security decisions and residual risks in clear everyday English.

## Work with related skills

- Use `authentication-engineering` for identity, sessions, tokens, OAuth/OIDC, MFA, and authorization design. This skill addresses threat context and broader controls around those flows.
- Use `backend-architecture` for backend modules and dependency direction; add security boundaries without imposing a new structure unnecessarily.
- Use `database-engineering` for detailed data models, transactions, migration safety, and database operations; this skill supplies security requirements for them.
- Use `engineering-workflow` for substantial phased implementation, verification, documentation, and Git checkpoints.
- Use `debugging-workflow` when the primary goal is to find the cause of a security defect or failed control.
- Use `plain-english-explainer` when a user wants a fuller lesson; keep routine security reports concise and understandable by default.

## Invocation examples

- Use `$security-engineering` to review this Express upload endpoint for injection, authorization, and unsafe file handling.
- Use `$security-engineering` with `$authentication-engineering` to threat-model this login and account recovery flow.
- Use `$security-engineering` with `$engineering-workflow` to harden this Docker deployment and add regression checks.
- Use `$debugging-workflow` to investigate this suspected SSRF issue, then use `$security-engineering` to assess the boundary and verify the fix.

## Scope and limitations

OWASP Top 10 lists are awareness aids, not complete risk assessments or compliance requirements. Check current standards, framework behavior, provider guidance, and jurisdictional obligations for the target system. This skill provides engineering guidance; it is not a compliance certification, authorized penetration test, or guarantee that a system has no vulnerabilities.

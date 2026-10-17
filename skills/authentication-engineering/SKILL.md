---
name: authentication-engineering
description: Design, implement, test, and maintain secure authentication and authorization systems. Version 1 gives language-independent principles with detailed initial guidance for Node.js, TypeScript, Express, Fastify, Next.js, PostgreSQL, MongoDB, and Redis. Use when a task changes identity, login, sessions, tokens, federation, MFA, or access decisions.
---

# Authentication Engineering

Use this skill when a system must establish who a caller is, maintain that identity across requests, or decide what the caller may do. Keep authentication (verifying identity) and authorization (checking access to an action or resource) distinct. Choose the smallest approach that meets the product's security, user, operational, and regulatory requirements.

Do not build custom authentication by default. First assess a maintained identity provider or framework library that fits the application and deployment. Prefer one when it safely covers needed flows and reduces protocol, account-recovery, and session-management work. Implement in-house only for a concrete requirement and with maintained protocol/cryptographic libraries; never invent cryptographic algorithms or protocols.

Read [identity and sessions](references/identity-and-sessions.md), [federation and advanced authentication](references/federation-and-advanced-auth.md), [authorization and framework integration](references/authorization-and-frameworks.md), and [testing and operations](references/testing-and-operations.md) as relevant. These are implementation references, not mandatory architecture layers.

## Workflow

1. Inspect the existing provider/library, identity model, login and recovery flows, session/token handling, authorization checks, framework lifecycle, and security tests before changing anything. Preserve working controls and existing conventions.
2. Identify protected assets, user groups, attackers, trust boundaries, client types, tenant boundaries, sensitive actions, availability needs, and realistic abuse cases. State unknowns and risk assumptions.
3. Choose the authentication model for the client and threat model: managed provider/library, server-side session, or tokens only where independently useful. Design login, recovery, logout, reauthentication, revocation, and account lifecycle together.
4. Define server-enforced authorization rules close to the protected operation/data. Check function permission, tenant, object ownership, and property access where each applies.
5. Use secure defaults and established maintained libraries. Keep credentials out of code, logs, and responses; minimize personal data and privilege.
6. Add regression tests for success and relevant negative paths. Test the actual framework/provider integration in an isolated environment when available; mocks alone cannot establish protocol or cookie behavior.
7. Review abuse, recovery, token/session lifecycle, race conditions, and operational visibility. Explain important decisions and trade-offs in plain English, and distinguish implemented, tested, untested, and recommended protections.
8. Use `engineering-workflow` for substantial implementation and checkpoints, `backend-architecture` for module boundaries, `database-engineering` for identity persistence and data safety, `debugging-workflow` for authentication defect investigation, `security-engineering` for broader application threats, and `plain-english-explainer` for detailed explanations. Each skill retains its scope.

## Non-negotiable engineering standards

- Authentication is not authorization. A valid login does not grant access to every tenant, object, operation, or field.
- Enforce access decisions on the server. Frontend route guards and hidden controls improve user experience but are not security controls.
- Use least privilege, secure defaults, and maintained providers/libraries. Avoid unnecessary complexity for small applications.
- Never store plaintext passwords, hardcode secrets, expose credentials, or invent custom cryptography.
- Protect against account enumeration, credential stuffing, brute force, phishing, session fixation, token replay, and unsafe recovery paths in proportion to risk.
- Do not weaken existing controls without explicit authorization and a documented reason. Require explicit authorization for production identity migrations or destructive account changes.
- Do not access or test systems outside the authorized scope. Do not run intrusive security testing without explicit authorization.
- Add relevant security regression tests. Do not claim protections or correctness that were only proposed or reviewed statically. Security review can reduce risk but cannot prove an application is completely secure.
- Explain the choice, important trade-offs, and verification results in plain English without discarding accurate technical terms.

## Standards and current guidance

Use the current versions relevant to the target application and jurisdiction. The OWASP Top 10 is an awareness list, not a complete requirements checklist. At the time this skill was written, OWASP lists its Web Top 10:2025 and API Security Top 10:2023; check their current editions during security reviews. Use OWASP Authentication, Password Storage, Session Management, Forgot Password, MFA, and CSRF cheat sheets for detailed implementation guidance. Use the OAuth 2.0 Security Best Current Practice and OpenID Connect Core for protocol requirements, NIST SP 800-63B-4 where assurance guidance applies, and the framework's current authentication documentation.

## Invocation examples

- Use `$authentication-engineering` with `$backend-architecture` to add email/password login to this Express API using an established library and its existing modules.
- Review this Next.js OAuth/OIDC callback for state, nonce, redirect, and ID-token validation.
- Use `$authentication-engineering` and `$database-engineering` to design refresh-token rotation and revocation storage.
- Use `$debugging-workflow` to investigate this login regression, then summarize confirmed behavior with `$plain-english-explainer`.

## Scope and limitations

Version 1 focuses on Node.js, TypeScript, Express, Fastify, Next.js, PostgreSQL, MongoDB, and Redis. Principles apply elsewhere, but library APIs and provider requirements vary and change. This skill is design guidance, not a compliance certification, penetration test, identity proofing standard, or substitute for current provider documentation and threat-specific expert review.

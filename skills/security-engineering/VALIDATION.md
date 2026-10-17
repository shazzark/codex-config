# Security Engineering Scenario Reviews

These are design reviews of the skill's guidance, not executable security tests or findings about a real application. No live target, credentials, repositories other than this skill source, or production environment were tested.

## 1. Express authentication API

**Scenario:** Review a Node.js/TypeScript Express API with email/password login, cookies, and PostgreSQL.

**Review result:** The skill routes identity-specific design to `authentication-engineering`, while requiring threat assessment, server-side authorization, abuse protection, CSRF review, safe error/log behavior, dependency/config checks, and tests at Express routes and persistence boundaries. Expected tests include invalid credentials, account enumeration behavior, rate limits, session invalidation, denied access, and injection-shaped input.

**Assessment:** Appropriate separation and actionable security checks; design exercise only. No API code or runtime test was executed.

## 2. Multi-tenant API authorization

**Scenario:** A REST API uses organization roles and exposes project records and exports.

**Review result:** Require authorization at every relevant endpoint and data operation, including list/search/export and background paths. Derive tenant context from trusted membership, scope queries, validate fields to prevent mass assignment, and test same-role cross-tenant and object/property access attempts.

**Assessment:** Covers object, function, and property-level boundaries without treating role names or unguessable IDs as sufficient; design exercise only. No API or database test was executed.

## 3. React/Next.js application

**Scenario:** A Next.js app renders user-specific data, uses Server Actions and cached fetches, and contains a React client interface.

**Review result:** Keep secrets server-only; validate runtime input despite TypeScript; authorize at each protected server entry point/data boundary; do not rely on UI or Proxy-only checks. Review output encoding/unsafe HTML, CSRF where cookie credentials apply, CORS, redirects, rewrites, cache audience and private-data serialization.

**Assessment:** Covers multiple framework entry points and cache leakage; design exercise only. No browser or Next.js runtime test was executed.

## 4. Docker and CI/CD configuration

**Scenario:** A Dockerized Node service is built by CI and deployed with environment-injected credentials.

**Review result:** Check maintained image versions, non-root execution, capabilities, filesystem/resources, secret leakage into build context/layers/logs, dependency and image scanning, CI token/job permissions, untrusted pull-request boundaries, deployment access, TLS/proxy trust, and rollback behavior.

**Assessment:** Provides deployable checks without demanding a specific cloud or pipeline vendor; design exercise only. No Docker build, workflow scan, or deployment was run.

## 5. Vulnerable dependency

**Scenario:** A scanner reports a vulnerable transitive Node dependency used by a production API.

**Review result:** Inspect the advisory and affected range, dependency path, installed version, reachable code path, fixed releases, compatibility, and mitigating configuration. Choose a supported fix, inspect lockfile changes, run relevant regression tests, and report remaining uncertainty rather than suppressing the alert or upgrading everything blindly.

**Assessment:** Balances evidence, impact, and focused remediation; design exercise only. No package audit, dependency installation, or vulnerability reproduction was executed.

## Overall result and limitations

All five scenarios were reviewed for technical accuracy, scope, actionability, and evidence-aware reporting. They show how the guidance should steer decisions but do not demonstrate runtime security behavior. No security tests, scans, penetration tests, or application changes were performed. Framework versions, threat exposure, regulatory obligations, and provider configuration must be evaluated for each target system.

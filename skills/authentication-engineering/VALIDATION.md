# Scenario Validation

The following are structured design reviews against this skill and references. They are not executable authentication tests, penetration tests, or claims that an application was changed. No credentials or live identity provider were used.

## 1. Email/password authentication for an Express API

**Scenario:** An existing Express API needs registration and login. It already uses route-level middleware and PostgreSQL, and has no special compliance requirement.

**Guidance outcome:** First inspect its current auth code and session conventions. Prefer a maintained provider/library if it fits the product; otherwise use a maintained password-hashing library and session middleware rather than writing cryptography. Validate input at the route, use generic login/reset responses, rate-limit abuse, hash passwords with an adaptive algorithm, and persist a server-side session. Regenerate the session ID on login, set secure cookie attributes, add CSRF protection for cookie-authenticated state changes, and use explicit route authorization. Test fixation, invalid credentials, enumeration parity, logout revocation, and protected-route denial.

**Evaluation:** Uses the existing middleware architecture, separates login from session and authorization, and favors a maintained library. Design review only; no Express code or runtime tests were run.

## 2. OAuth/OIDC login for Next.js

**Scenario:** A Next.js application adds enterprise OIDC login with a server-rendered dashboard.

**Guidance outcome:** Use a maintained Next.js-compatible OIDC/auth library. Use Authorization Code with PKCE, one-time session-bound state, nonce validation, exact registered callback URI, and full ID-token issuer/audience/signature/time validation. Create an application session server-side with secure cookies. Perform authorization in server actions/route handlers/data access close to private data; client route guards are only a UI convenience. Do not auto-link accounts solely by email.

**Evaluation:** Correctly distinguishes OAuth delegated access from OIDC authentication and covers callback binding and server-side authorization. Design review only; no provider configuration or callback test ran.

## 3. JWT refresh-token rotation and revocation

**Scenario:** A service needs API access tokens plus long-lived sessions across devices, with logout and reuse detection.

**Guidance outcome:** Confirm JWT is needed; an opaque server session may be simpler. If using JWT access tokens, keep them short-lived and validate algorithm, key, issuer, audience, type, and time claims. Keep refresh tokens high-entropy and server-tracked (store a hash), rotate atomically, associate tokens with a family/device session, and detect reuse to revoke that family. Define simultaneous refresh and lost-response behavior. Add revocation/version checks when immediate access-token revocation matters.

**Evaluation:** Does not equate signed JWTs with revocability, and identifies atomicity and replay as persistence/test concerns. Design review only; no token implementation or concurrency tests ran.

## 4. RBAC and resource authorization in a multi-tenant API

**Scenario:** An API has organization roles and resources owned by individual projects within each organization.

**Guidance outcome:** Use roles for stable broad permissions, then enforce tenant membership, action permission, and object/project ownership server-side for every read/write/list/export path. Scope database queries by trusted tenant context, prevent client-controlled tenant/role mass assignment, and test same-role cross-tenant access plus property-level privilege changes. Keep UI visibility checks as convenience only.

**Evaluation:** Targets broken object-level authorization and cross-tenant paths instead of assuming role checks are sufficient. Design review only; no API tests ran.

## 5. Password reset, recovery, and MFA

**Scenario:** Users can reset passwords by email, enroll TOTP MFA, and use recovery codes if a device is lost.

**Guidance outcome:** Use generic reset responses, a short-lived single-use random token stored hashed, and a configured trusted origin for links. Revoke or rotate sessions after reset. Protect TOTP seeds as secrets; store recovery codes hashed and consume once. Rate-limit reset and OTP attempts; notify on security changes. Make MFA recovery at least as carefully controlled as enrollment, and do not let email-only reset silently disable MFA. Test expired/replayed tokens, enumeration, OTP replay/limits, recovery-code reuse, session revocation, and unauthorized MFA removal.

**Evaluation:** Covers the weakest-link risk of recovery and includes negative-path regression tests. Design review only; no mail delivery or MFA runtime tests ran.

## Results and limitations

All five exercises pass the architecture, security, maintainability, and testability review criteria. They are design exercises informed by OWASP, IETF, OpenID Foundation, NIST, and Next.js documentation. No executable tests, provider integration checks, browser tests, scans, or penetration tests were performed. Exact library selection, settings, threat coverage, and production readiness require the actual application, current dependency/provider documentation, and authorized verification.

# Authentication Testing and Operations

## Security regression tests

Test the security contract, not only the happy path. Use isolated users, tenants, providers, and databases; avoid real credentials or production records.

- **Registration/login:** valid and invalid credentials, duplicate registration, unknown account versus wrong password response, disabled/unverified accounts, email-verification expiry/reuse and verification gating, malformed input, rate limiting, and lockout/recovery behavior.
- **Passwords/recovery:** hash verification and upgrade path, expired/used/replaced reset token, unknown-account response parity, token leakage prevention, password reset revocation, MFA recovery, and notification behavior.
- **Sessions/cookies:** session ID changes at login/privilege transitions, cookie flags and scope, idle/absolute expiry, one-session and all-session revocation, logout invalidation server-side, CSRF rejection on unsafe cookie-authenticated requests, and fixation attempts.
- **JWT/access/refresh:** bad signature, wrong algorithm/key, wrong issuer/audience/type, expired/not-yet-valid token, revoked session, refresh rotation, concurrent refresh, reuse/replay detection, and logout/revocation.
- **OAuth/OIDC:** state mismatch/replay, PKCE mismatch, nonce mismatch, wrong issuer/audience/signature, expired ID token, callback error, unregistered redirect, open redirect, provider-key rotation, and account-linking proof.
- **Authorization:** unauthenticated access, role/function denial, user A accessing user B's object, cross-tenant list/read/write/export, field-level privilege escalation, mass assignment, GraphQL resolver access, and background-job authority.
- **MFA/passkeys:** invalid/expired/replayed challenge, OTP attempt limit and single use, recovery-code reuse, unauthorized factor removal, wrong WebAuthn origin/RP ID, and step-up expiry.
- Use integration tests with the real session store/database/provider test mode for cookie, transaction, and protocol behavior. Mocks are useful for unit policy but cannot prove provider configuration or browser cookie behavior.

## Abuse protection and error handling

- Apply bounded rate limits and abuse detection to login, registration, verification, reset, MFA, token refresh, and recovery. Use multiple signals while accounting for shared NATs and denial-of-service risks from hard account lockout.
- Keep responses and timing reasonably consistent where account existence is sensitive. Do not leak whether an email, username, tenant, MFA enrollment, or reset token exists. Avoid excessive timing-equalization complexity without a threat-based reason.
- Return generic authentication failures and safe authorization errors. Keep detailed causes in access-controlled logs with request/correlation IDs; never log passwords, full tokens, cookies, OTPs, recovery codes, or reset links.
- Alert on meaningful patterns such as credential stuffing, repeated recovery, MFA changes, impossible privilege changes, and session/token reuse. Define retention and access controls for authentication events.

## Configuration and operations

- Load client secrets, session-signing keys, token-signing keys, encryption keys, and provider credentials from a secret manager or protected deployment configuration. Support rotation with a documented overlap plan; do not commit or print secrets.
- Use HTTPS end-to-end as appropriate, correctly configure trusted reverse proxies, and trust forwarded headers only from known proxies. Incorrect proxy trust can break secure-cookie decisions or enable spoofing.
- Restrict provider redirect URIs and origins to known values. Separate development and production credentials/clients. Review callback host, cookie domain, clock synchronization, key rotation, and fail-closed behavior during deployment.
- Keep dependencies maintained and monitor advisories. Review security-sensitive configuration and library versions as part of release work.
- Treat production identity migrations and destructive account changes as high-impact: require explicit authorization, scope, rollback/forward-repair plan, backup where appropriate, and post-change verification.
- Monitor authentication availability without logging secrets. A provider outage should fail safely and predictably; do not silently bypass MFA, token checks, or authorization to improve availability.

## Evidence and reporting

For each finding, record the affected flow/component, threat precondition, observed behavior, impact, evidence, confidence, and focused remediation. Separate confirmed vulnerabilities from hypotheses and general hardening suggestions. State which tests, scans, configuration checks, and environments were actually used. A clean scan or passing suite does not prove the identity system is secure.

## References

- [OWASP Top 10:2025](https://top10.owasp.org/2025/)
- [OWASP API Security Top 10:2023](https://api-security.owasp.org/editions/2023/en/0x00-header/)
- [NIST SP 800-63B-4](https://pages.nist.gov/800-63-4/sp800-63b.html)
- [OAuth 2.0 Security Best Current Practice, RFC 9700](https://www.rfc-editor.org/rfc/rfc9700.html)
- [Next.js Authentication Guide](https://nextjs.org/docs/app/guides/authentication)

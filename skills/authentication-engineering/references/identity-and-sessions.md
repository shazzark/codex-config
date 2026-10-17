# Identity, Accounts, Sessions, and Tokens

## Account lifecycle and passwords

- Separate a user/person record from authentication methods when users may use passwords, multiple identity providers, passkeys, or several email addresses. Store provider identity as the pair `(issuer, subject)`; email addresses can change and may not be verified.
- Registration should validate input, apply abuse limits, and avoid exposing whether an account exists when that enables enumeration. Treat email verification as evidence of mailbox control, not proof of a real-world identity.
- Store only password hashes from a maintained password-hashing library. Prefer Argon2id for new systems; use scrypt when Argon2id is unavailable or unsuitable. Reserve bcrypt for legacy compatibility when those options are unavailable. Do not use fast general-purpose hashes such as SHA-256 for passwords. Let the library handle salts and hash formats, benchmark a suitable work factor, and support safe upgrades.
- If bcrypt must be used, verify the chosen library's password input limit and behavior. Most implementations accept at most 72 bytes, so reject longer inputs clearly instead of silently truncating them; count encoded bytes, not characters. Do not add custom pre-hashing without a reviewed, established design.
- Follow current guidance for password length, breached/common-password screening, and input handling. Avoid arbitrary composition rules or periodic resets unless risk or policy requires them. Never log, email, or display passwords.
- Verify credentials using the library's safe verification function. Apply rate limits by several signals (for example, account identifier and network source) with progressive controls that do not let an attacker permanently lock out a victim. Keep login and recovery responses generic enough to prevent account enumeration while remaining usable.
- Email verification and password-reset tokens should be cryptographically random, purpose-bound, short-lived, single-use, and stored in a form that does not reveal the bearer token if the database is read. Invalidate them after use or replacement; do not put them in logs or third-party referrers.
- A reset request should return a consistent response for existing and unknown accounts. Send the link through a trusted channel, use a configured canonical origin rather than an untrusted Host header, and avoid changing account state until the token is proved. After reset, revoke or rotate sessions and refresh credentials according to risk. Account recovery must not silently bypass MFA; provide recovery codes and audited factor reset with comparable protection.
- Define account disablement, deletion, identity unlinking, and retention behavior. Revoke active sessions/tokens when access should end. Preserve only data required by explicit business/legal rules.

## Server-side sessions and browser cookies

- A session cookie is a bearer credential: anyone holding it can act as that user until it expires or is revoked. Use unpredictable session IDs and keep session data server-side when immediate revocation, device management, or mutable authorization state matters.
- Regenerate the session identifier after login, privilege change, and other trust-level changes to prevent session fixation. Set idle and absolute expiry according to risk; rotate credentials after password reset, account recovery, MFA changes, and suspected compromise.
- For HTTPS web apps, set session cookies `HttpOnly`, `Secure`, an appropriate `SameSite` value, narrow `Path`/host scope, and explicit expiry. Do not expose session secrets to client JavaScript or URLs. `SameSite` is defense in depth, not a universal replacement for CSRF protection on cookie-authenticated state changes.
- Protect cookie-authenticated unsafe requests with framework-supported CSRF protection or an appropriate token/origin strategy. Keep state changes off `GET`. Configure CORS for the specific trusted origins; CORS is not authorization and does not stop non-browser callers.
- Use maintained session middleware with a production session store. Do not use an in-memory development store in a multi-instance production deployment. Redis-backed stores need protected network access, access controls, TTL, availability expectations, and deliberate behavior during outage/eviction.
- Give users and support staff a safe way to review and revoke active sessions/devices when the product requires it. Show only useful, privacy-conscious metadata (such as last activity and coarse device label); do not treat IP address or user-agent as a reliable device identity.
- Logout should invalidate the server session and expire the browser cookie with matching scope. Clearing the cookie alone does not revoke a copied session ID.

## Access tokens and refresh tokens

- Use tokens when the client/API boundary needs them; a JWT is not automatically safer or simpler than a server-side session. For a same-site web app, an opaque server-side session or a backend-for-frontend can keep bearer tokens out of browser storage.
- Keep access tokens short-lived and narrowly scoped. Never put secrets or unnecessary personal data in a JWT: signed JWT payloads are generally readable by their holders. Validate the signature using an explicit allowed algorithm and trusted key, and check issuer, audience, expiry, not-before, token type, and relevant scopes/claims. Never trust client-selected algorithms or decode without verification.
- Refresh tokens are long-lived bearer credentials. Prefer opaque high-entropy refresh tokens stored hashed server-side when revocation and replay detection are needed. Rotate on use, bind tokens to a family/session, atomically consume the old token, and detect reuse; revoke the family or session after detected replay. Store client credentials securely.
- JWT revocation is not automatic. Use short access lifetimes plus a server-side session/version/denylist check where immediate revocation is required, and account for the trade-off between state and delayed revocation.
- Logout and account/security events should revoke the refresh session and invalidate other credentials according to policy. Test simultaneous refreshes and retry behavior so a legitimate network retry does not accidentally create two valid token branches.
- Do not store bearer or refresh tokens in `localStorage` or `sessionStorage` by default; any same-origin script can read them. Select a browser pattern with current framework guidance and explain residual XSS/CSRF risks.

## References

- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [OWASP Forgot Password Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html)
- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
- [OWASP CSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
- [OWASP Multifactor Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html)
- [OWASP Credential Stuffing Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Credential_Stuffing_Prevention_Cheat_Sheet.html)

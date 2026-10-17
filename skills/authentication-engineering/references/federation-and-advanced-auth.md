# OAuth, OpenID Connect, MFA, and Passkeys

## OAuth 2.0 and OpenID Connect

- OAuth 2.0 is an authorization framework for delegated access. OpenID Connect (OIDC) adds an identity layer. Use OIDC when the application needs to authenticate a user through an external identity provider; an OAuth access token alone is not proof of the user's identity.
- Prefer Authorization Code flow with PKCE for browser/native clients and follow the provider's current guidance. Use an established OIDC client library. Do not implement protocol details by hand.
- Generate and validate one-time, unpredictable `state` bound to the initiating client/session transaction to defend the callback against request forgery. Use `nonce` when required by the flow/profile; whenever it is sent, compare the returned ID-token claim to the original value. Keep the PKCE verifier in the initiating client/session context for the short-lived transaction and verify the returned code against it.
- Match registered redirect URIs as required by the provider and client profile, including any narrowly defined native-loopback exception; do not accept arbitrary patterns or return URLs. Use a local allowlist for post-login redirects. Reject open redirects and callbacks with missing, mismatched, expired, or reused state.
- Validate ID-token signature from the trusted issuer's current keys and validate `iss`, `aud`, expiry/time claims, and `nonce` when present/required. Handle key rotation from trusted discovery metadata and pin expected issuer/client configuration. Do not trust claims merely because they are in a JWT-shaped string.
- Validate provider access tokens at the resource server using the provider's supported method (for example, issuer keys or introspection), expected audience, expiry, and required scopes. Do not accept an ID token as an API access token or treat an opaque access token as locally verifiable without provider support.
- Keep provider access/refresh tokens separate from the application's own session. Request minimum scopes, store provider tokens only if needed, protect them as secrets, and support revocation/unlinking where available.
- Do not automatically link an external login to an existing account based only on a matching email. Require an authenticated recent session or prove control of both identities; make linking/unlinking explicit and auditable.
- Handle account creation, provider errors, callback retries, cancellation, and partial failures without logging codes, state, tokens, or ID-token contents.

## Multi-factor authentication and step-up

- MFA requires independent factors; two passwords/PINs are still one factor. Offer stronger phishing-resistant options such as WebAuthn/passkeys when appropriate. OTP and push methods have different phishing/replay properties; describe those accurately instead of calling all MFA phishing-resistant.
- Bind, replace, and remove authenticators only after appropriate recent authentication. Store TOTP seeds as secrets protected at rest; hash one-time recovery codes and consume them once. Apply expiry and strict attempt limits to OTPs.
- Treat recovery and factor reset as high-risk authentication flows. Notify users of changes, protect support/admin override paths, and revoke other sessions when risk warrants it.
- Require step-up/re-authentication for high-impact operations (such as changing email/password, removing MFA, changing payout details, or exporting sensitive data) based on risk. A recent login indicator in the UI is not enough; verify freshness server-side.
- Keep an event history for login, failed attempts, factor binding/removal, recovery, account linking, session revocation, and privileged changes. Log actor, outcome, time, and safe context; never log secrets, OTPs, full tokens, or recovery codes.

## WebAuthn and passkeys

- Prefer a maintained WebAuthn implementation and follow the current WebAuthn/FIDO protocol and browser guidance. The server stores public-key credential data and verifies signed challenges; it does not receive the passkey's private key or biometric.
- Generate fresh unpredictable challenges and bind them to the user/session and ceremony. Validate challenge, origin, RP ID, credential ownership, ceremony type, and required user-presence/verification flags using the library. Challenges must expire and be single-use.
- Support multiple credentials and safe enrollment/removal. Require reauthentication or another strong factor to add a credential, provide recovery, and notify users of security-sensitive changes.
- Do not claim passkeys eliminate every risk: account recovery, device loss, phishing of fallback flows, and authorization policy still matter.

## References

- [OAuth 2.0 Security Best Current Practice (RFC 9700)](https://www.rfc-editor.org/rfc/rfc9700.html)
- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)
- [Proof Key for Code Exchange (PKCE), RFC 7636](https://www.rfc-editor.org/rfc/rfc7636.html)
- [NIST SP 800-63B-4: Authentication and Authenticator Management](https://pages.nist.gov/800-63-4/sp800-63b.html)
- [W3C Web Authentication (WebAuthn)](https://www.w3.org/TR/webauthn-3/)

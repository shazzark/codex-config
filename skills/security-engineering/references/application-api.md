# Application and API Security

## Risk framing

Use the current [OWASP Web Top 10](https://top10.owasp.org/2025/) and [OWASP API Security Top 10](https://api-security.owasp.org/editions/2023/en/0x00-header/) to discover common risk areas, then assess the actual application. Relevant issues include broken access control, security misconfiguration, supply-chain failures, cryptographic failures, injection, insecure design, authentication failures, integrity failures, logging failures, and mishandling exceptional conditions. API risks also include object/property/function authorization failures, resource exhaustion, sensitive business-flow abuse, SSRF, inventory gaps, and unsafe consumption of other APIs.

Threat-model the application as assets, actors, trust boundaries, entry points, and abuse cases. Include browser-to-server, service-to-service, worker-to-queue, application-to-database, and outbound network paths where they exist. Prioritize by likelihood, impact, exposure, and available mitigations; do not apply the Top 10 mechanically.

## Input, output, and injection

- Validate inputs at the boundary using an allowlist of accepted shapes, types, ranges, and formats. Validation does not replace authorization, contextual output encoding, or safe query construction.
- Use parameterized SQL and safe query-builder APIs. Never concatenate untrusted values into SQL syntax. Allowlist identifiers such as sort columns when they cannot be parameters.
- For MongoDB, construct filters from validated fields and typed values. Do not pass request objects directly as query operators or allow untrusted `$where`/operator expressions.
- Treat data from integrations, stored records, and queues as untrusted when it crosses into a new parser, query, template, or command context.
- Encode output for its destination. React escapes ordinary text by default, but unsafe HTML APIs such as `dangerouslySetInnerHTML`, URL sinks, and direct DOM operations require explicit review and safe handling.
- Avoid shell commands with interpolated input. Prefer APIs that pass arguments separately; if a process is necessary, validate arguments and constrain its privileges and environment.

## Browser and API boundaries

- Prevent XSS by avoiding unsafe HTML, encoding for the right context, and using a considered Content Security Policy (CSP) as defense in depth. A CSP does not make unsafe rendering safe.
- For cookie-authenticated state changes, assess CSRF and use a suitable framework-supported token or origin-checking defense. `SameSite` cookies help but are not a universal CSRF solution. Keep state changes off `GET`.
- Set CORS to the specific browser origins and credential behavior required. CORS is not authentication or authorization and does not constrain non-browser clients.
- Configure security headers and transport protections for the real hosting topology. Avoid copying headers blindly when they conflict with application behavior.
- Enforce request size, time, concurrency, and rate limits according to resource cost and abuse risk. Bound expensive searches, uploads, exports, and business flows.
- Return safe errors to callers. Keep detailed causes in access-controlled logs without secrets, tokens, personal data, or stack traces exposed to untrusted clients.

## SSRF, redirects, and file handling

- For server-side fetches, avoid destinations derived from untrusted URLs when possible. Otherwise allowlist schemes, hosts, and ports, resolve and re-check destinations as appropriate, block unintended private/loopback/link-local/metadata ranges, control redirects, set timeouts and response-size limits, and constrain outbound network access. Account for DNS changes and proxy behavior in the actual environment.
- Use a local allowlist for redirect targets; never send users to arbitrary destinations from query/header input.
- For uploads, enforce size and count limits, inspect actual content rather than trusting filename or client MIME type, generate storage names, and store outside executable/web-root locations or in isolated object storage. Consider malware scanning or quarantine when the risk warrants it.
- Authorize downloads independently, prevent path traversal, set safe content-disposition and content-type behavior, and avoid serving active content from a trusted application origin when isolation is appropriate.

## Framework guidance

- **Express:** Set explicit body and upload limits; use maintained middleware with deliberate ordering; configure `trust proxy` only for known proxies; centralize safe error handling; attach controls to every relevant router and route. Avoid trusting request headers as identity or origin without a trusted boundary.
- **Fastify:** Use schema validation/serialization where appropriate, but verify that schemas do not accidentally strip or expose security-sensitive fields. Apply authentication, authorization, rate limits, and hooks to the exact encapsulated routes. Keep error handlers from converting a denial into success.
- **React:** Treat browser state as attacker-controlled. Do not put secrets in bundles or client-visible props. Avoid raw HTML and unsafe URL handling; server APIs must independently authorize requests.
- **Next.js:** Identify every server entry point, including Route Handlers, Server Actions, server-rendered data access, and Proxy/Middleware. Enforce authorization at the protected operation or data boundary; routing checks alone are not sufficient. Keep secrets server-only, prevent private data from entering public props/cache, scope cached results to their audience, and review `next.config.*`, rewrites, image remote patterns, and source maps.
- **TypeScript:** Types help prevent programming mistakes but do not validate runtime input. Parse and validate data received from HTTP, environment variables, queues, databases, and external APIs.

## Authentication and authorization boundary

Ensure endpoints verify identity where required and authorize the requested action on the specific object, tenant, and fields. Apply checks to list/search/export and background paths as well as direct reads and writes. Do not assume an unguessable identifier, frontend guard, CORS policy, or role name grants access. For credential, session, OAuth, MFA, recovery, or detailed authorization design, use `authentication-engineering` rather than duplicating its full flow guidance here.

## References

- [OWASP Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [OWASP CSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
- [OWASP SSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html)
- [OWASP File Upload Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/File_Upload_Cheat_Sheet.html)
- [OWASP Next.js Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Nextjs_Security_Cheat_Sheet.html)

# Authorization and Framework Integration

## Authorization policy

- Make authorization an explicit server-side decision: who is acting, what action they request, on which resource, in which tenant/context, and under what current policy.
- Use RBAC (role-based access control) when stable job/organization roles map cleanly to permissions. Prefer named permissions for fine-grained actions. Use ABAC (attribute-based access control) only when decisions genuinely depend on attributes such as ownership, tenant, classification, or time; keep policies understandable and testable.
- Roles in a session or token can become stale. Refresh high-impact privileges from an authoritative server-side source or use short-lived/versioned claims. Do not let user-editable profile fields grant privileges.
- Check object-level access on every path that accepts or resolves an object ID, including list, search, export, GraphQL resolver, batch, admin, and background-job paths. Scope the query to authorized tenant/owner where possible, and do not treat an unpredictable ID as authorization.
- Enforce tenant scope in data access and mutations, not only in route middleware. Derive tenant context from trusted authenticated membership, check that membership is current, and include tenant keys in uniqueness/relationships where appropriate. Prevent cross-tenant reads, writes, counts, and error side channels.
- Return only authorized fields. Prevent mass assignment of role, tenant, ownership, or privilege fields from client input. GraphQL field resolvers must enforce object/property authorization; hiding a field in the frontend is insufficient.
- Default deny for sensitive actions, deny when policy dependencies fail, and keep policy checks close to the operation. Avoid broad catch/fallback behavior that accidentally grants access.
- Record policy decisions at useful security events without logging sensitive values. Authorization failures should reveal no more resource information than product behavior requires.

## Express and Fastify

- Use established, maintained authentication/session/OIDC middleware or providers where suitable. Order middleware/hooks so request parsing limits and identity establishment precede protected handlers, and authorization applies before protected reads or writes.
- In Express, keep authentication and authorization middleware explicit per route/router; central error handling must not convert denial or provider failure into success. Check async error behavior for the installed Express version.
- In Fastify, register authentication as an encapsulated plugin/decorator and apply `preHandler` or route-level hooks deliberately. Plugin scope must cover exactly the routes that depend on it. Avoid global mutable identity state.
- Ensure background jobs and service-to-service calls use explicit service identity and scoped permissions; browser session middleware does not authenticate a worker.

## React and Next.js

- Client-side route guards, React context, navigation redirects, and hidden controls are user-interface conveniences only. Enforce access again in server actions, route handlers/API endpoints, server components that fetch private data, and the data-access layer.
- In Next.js, keep secrets and session verification server-only. Use the framework's current cookie APIs and an established auth library/provider where suitable. Set appropriate `HttpOnly`, `Secure`, `SameSite`, expiry, and scope attributes on cookies.
- Middleware/Proxy can make optimistic redirects, but do not make it the only protection for private data or privileged operations. Check authorization close to the database or protected action, including Server Actions and Route Handlers.
- Use a backend-for-frontend (BFF) or framework-supported server session when it avoids exposing long-lived tokens to browser JavaScript. Review CSRF, XSS, caching, prefetch, and cross-origin behavior for the chosen flow.
- Never serialize access tokens, refresh tokens, password hashes, MFA secrets, or internal authorization data into client props, React context, HTML, logs, or source maps.

## PostgreSQL, MongoDB, and Redis

- **PostgreSQL:** Persist users, external identities, memberships, roles/permissions, sessions, and token families with explicit keys, uniqueness, and foreign-key constraints where appropriate. Store tenant membership explicitly. Use transactions for coupled state changes such as consuming a refresh token and issuing its successor. Apply authorization filters in SQL queries as well as service policy.
- **MongoDB:** Model each user's credentials/identity links and tenant membership according to access patterns and ownership. Use unique indexes and atomic conditional updates for one-time token consumption and refresh rotation. For invariants spanning documents, use a transaction only when the deployed topology supports it and the invariant requires it.
- **Redis:** Use Redis for session storage, short-lived challenges, throttling, or revocation data only with explicit TTL, persistence/eviction expectations, access controls, and outage behavior. Redis eviction or failover must not silently restore revoked privilege or make a one-time code reusable. Do not use a cache as the only source for critical authorization unless its consistency and failure semantics are deliberate.

## References

- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [OWASP API Security Top 10:2023](https://api-security.owasp.org/editions/2023/en/0x00-header/)
- [Next.js Authentication Guide](https://nextjs.org/docs/app/guides/authentication)
- [OWASP CSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)

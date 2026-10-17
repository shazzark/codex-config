# Database, Cache, Queue, and Integration Security

## Databases

- Use `database-engineering` for schema, transaction, migration, query-plan, backup, and engine-specific decisions. Apply security controls to those designs: least-privilege application accounts, separate migration/admin credentials, protected network paths, and no production credential sharing with development or CI.
- Use parameterized SQL and safe query APIs; validate identifiers that cannot be bound as parameters. For document databases, construct filters from validated fields and values rather than accepting raw query operators. Do not expose database errors or connection details to clients.
- Restrict database roles to required schemas, tables, operations, and environments. Use TLS where traffic crosses an untrusted boundary and encryption-at-rest features where the sensitivity and threat model warrant them.
- Protect backups, exports, replicas, test fixtures, and logs like the underlying records. Use synthetic or sanitized data outside production; limit retention and access to sensitive records.
- Review migrations and restore plans for security implications, including privilege, indexes that affect tenant isolation, data retention, and exposure of newly added fields.

## PostgreSQL and MongoDB

- **PostgreSQL:** Use separate roles for runtime and schema changes where practical. Parameterize query values, enforce constraints and row/tenant boundaries in the application or database policy as appropriate, and protect connection strings. Avoid granting broad owner/superuser rights to application processes.
- **MongoDB:** Enable authentication and network controls, scope database users to required databases/collections/actions, and avoid evaluating user-controlled query expressions. Validate object shapes and update operators. Protect tenant and ownership predicates on every query path.
- Database permissions and row-level policies can add defense in depth, but confirm the application's connection role and every access path actually use them as expected.

## Redis and caches

- Keep Redis off public networks; require authentication, use ACLs with narrow command/key access, and use TLS when traffic crosses an untrusted network. Keep credentials in protected configuration.
- Define cache keys, TTLs, invalidation, and outage behavior. Include tenant/user/permission context when values are not public. Never serve private cached data to a broader audience because a cache key omitted an authorization boundary.
- Treat cache entries as potentially stale. Re-check authorization for protected operations when policy changes or revocation must take effect immediately. Do not use evictable cache state as the sole record of a critical security decision without deliberate failure semantics.
- Avoid using Redis as a generic distributed-lock security guarantee. Understand expiry, failover, and partial failure limits; use a datastore-backed invariant or a well-reviewed coordination design when correctness depends on exclusive ownership.

## Queues and background workers

- Authenticate and authorize producers and consumers. Restrict who can publish, consume, inspect, purge, or retry messages; separate queues by trust level when needed.
- Validate message schemas and versions because queued data may outlive a deploy and may be produced by compromised or older components. Do not trust user-provided identity/tenant fields without re-establishing authority.
- Minimize sensitive payloads; prefer references to protected records where appropriate. Protect broker transport, credentials, dead-letter queues, dashboards, and retained messages.
- Make authorization explicit for worker actions and scope service credentials to required operations. Add idempotency, bounded retries, poison-message handling, and audit events without logging secrets or full sensitive payloads.

## External services and webhooks

- Validate external responses before using them in queries, templates, commands, or downstream requests. Apply TLS, timeouts, response-size limits, bounded retries, and redirect policy.
- Restrict outbound destinations where feasible to reduce SSRF and credential exfiltration. Keep provider credentials scoped and separate by environment; rotate exposed credentials.
- Verify webhooks using the provider's maintained signature scheme, compare signatures safely, enforce timestamp/replay limits where supported, and make processing idempotent. Do not trust an event because its payload names a trusted account.
- Define behavior during provider outage and partial failure. Do not bypass authorization, signature checks, or validation as an availability fallback.

## References

- [OWASP Database Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html)
- [OWASP NoSQL Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/NoSQL_Security_Cheat_Sheet.html)
- [OWASP Web Service Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Web_Service_Security_Cheat_Sheet.html)
- [OWASP Webhook Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Webhook_Security_Cheat_Sheet.html)

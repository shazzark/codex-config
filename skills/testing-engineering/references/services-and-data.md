# Services, contracts, and data

## HTTP APIs

Test the externally visible request/response behavior: status, headers, body shape, validation, authentication/authorization decisions, and relevant error semantics. Include successful and denied/invalid cases, boundary values, missing fields, malformed payloads, and not-found/conflict behavior where part of the contract. Do not assert internal helper call counts unless that interaction itself is the contract.

For Express, Supertest can exercise an app/server without requiring a public deployment. For Fastify, `app.inject()` sends an in-process request through Fastify's request lifecycle. Verify exactly which middleware, plugins, hooks, error handlers, and serialization paths are included; test reverse-proxy, TLS, or network behavior in a separate integration environment when relevant. See [Supertest](https://github.com/ladjs/supertest) and [Fastify testing](https://fastify.dev/docs/latest/Guides/Testing/).

## API and message contracts

Validate wire data at runtime where a schema is part of the boundary. Check provider responses against consumer expectations and version changes for compatibility. Contract tests are valuable for separately deployed consumers/providers, but should not recreate the entire implementation or assert undocumented internals. For queues/events, test schema compatibility, duplicate delivery handling, ordering assumptions, and error/dead-letter behavior as required by the system.

## Databases and caches

Use an isolated database instance/schema or unique test database with a compatible engine/version for behavior depending on SQL, query semantics, constraints, indexes, transactions, concurrency, or migrations. PostgreSQL tests should exercise actual constraints and transaction boundaries where they define correctness. MongoDB tests should use the actual query/update semantics and topology needed for transactions or indexes. Redis tests should use a real isolated Redis when expiration, atomic operations, Lua/scripts, pub/sub, queue, or eviction behavior matters.

Keep fixtures small and deterministic. Seed only necessary data. Use per-test rollback or unique test namespaces when supported; ensure parallel tests cannot collide. Test cleanup and connection closure. Do not point destructive test setup at a production URL, and verify the target identity before migrations or data resets. Never use production data as a fixture without explicit approval and an approved sanitization process.

Test failure and concurrency cases when they affect guarantees: constraint violations, transaction rollback, simultaneous updates, duplicate requests, lock timeouts, lost connection, cache miss/expiry/invalidation, and retry idempotency. A mock can test caller handling but cannot prove the engine's locking, isolation, or driver behavior.

Consult `$database-engineering` for schema/migration design, engine-specific safety, and operational rules. Consult `$authentication-engineering` and `$security-engineering` for required access-control and abuse cases, then encode the relevant observable denial behavior in tests.

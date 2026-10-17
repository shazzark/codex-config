# Node.js and TypeScript Patterns

Use these as adaptable patterns, not a required directory tree. Inspect the existing project before introducing roles or moving files.

## Choose a shape that fits

- A small API can use a few cohesive feature modules and a clear composition/startup entrypoint. Avoid adding domain, application, repository, adapter, and factory layers when they only forward calls.
- A growing application can group code by capability (for example, `users/`, `projects/`, `billing/`) and expose narrow module entrypoints. Keep cross-feature calls explicit.
- A worker process or separate service is justified by different scaling, failure isolation, resource profile, deployment cadence, ownership, or security requirements. A folder or class named `service` alone is not a reason to split deployment.
- Keep the composition root responsible for constructing adapters, connecting resources, registering routes/jobs, and starting/shutting down the process. Avoid import-time network connections and mutable global singletons where practical.

An illustrative feature could contain `routes.ts`, `schemas.ts`, `create-project.ts`, `project-store.ts`, and tests, or a different arrangement that fits local conventions. A trivial feature may need only one or two files. Shared `http/`, `config/`, or `infrastructure/` modules should contain reusable policy or adapters rather than unrelated feature logic.

## HTTP and application responsibilities

- **Routes** bind HTTP method/path, middleware, schema validation, and handler. Keep route registration declarative and make auth requirements visible.
- **Controllers/handlers** translate transport input into an application call and translate its result into HTTP status and representation. In small systems, the handler can be the route callback; do not add a controller wrapper that only forwards arguments.
- **Application services/use cases** coordinate a meaningful operation, enforce application policy, and call ports/adapters. They should not depend on framework request or response objects.
- **Domain modules** hold business invariants when the domain has meaningful rules. Do not invent a domain layer for straightforward CRUD with no independent policy.
- **Repositories/stores** express persistence operations when the boundary is useful. Avoid generic CRUD abstractions that hide query intent, transaction scope, or database capabilities.
- **Schemas/validators** define external input and, where useful, output contracts. Validate at the boundary and convert to trusted typed values. TypeScript types disappear at runtime; they do not validate JSON.
- **Infrastructure adapters** own database clients, cache clients, queue publishers, mailers, and external APIs. Keep vendor-specific details behind a boundary only when it clarifies substitution, testing, or ownership.

Prefer explicit function arguments or a small constructor to service locators and broad dependency containers. Avoid importing another feature's private files; expose a narrow public API or move truly shared policy into an owned module.

## Express and Fastify

In Express, compose middleware in a deliberate order: request IDs and safe logging, body parsing/limits, authentication, route-specific authorization and validation, handlers, then a centralized error handler. Ensure async failures reach the error handler for the installed Express version and use a final not-found handler. Avoid middleware that silently changes the meaning of request data.

In Fastify, use encapsulated plugins to scope routes, decorators, hooks, and schemas. Register shared plugins before dependent routes and use Fastify's schema/serialization capabilities consistently. Do not bypass encapsulation with hidden mutable global state. In either framework, handlers should produce consistent response and error contracts.

## Validation, errors, and security

- Validate params, query, headers, and body at the edge with runtime schemas; reject unknown fields where the contract calls for it. Normalize only deliberately. Bound payload sizes, string lengths, pagination, and expensive filter shapes.
- Represent expected outcomes (not found, conflict, forbidden, invalid state) distinctly from unexpected faults. Map them centrally to stable status codes and safe public error bodies. Keep stack traces and driver errors server-side.
- Authentication proves who the caller is; authorization checks whether that principal may perform this action on this resource and tenant. Apply authorization close to the operation, including background/admin paths as relevant. Never trust a user ID, role, or tenant claim supplied only in a request body.
- Use secure password hashing and established session/token libraries. Enforce cookie, CSRF, token expiry/rotation, and rate-limiting choices based on the actual auth model. Keep secrets out of logs, errors, source files, and client responses.
- Use parameterized database operations, least-privilege database identities, secure transport, safe CORS policy, and dependency maintenance. Avoid logging credentials, tokens, payment data, or unnecessary personal data.

## PostgreSQL

- Represent invariants in schema constraints (foreign keys, unique/check constraints, nullability) when appropriate; application checks alone race under concurrency.
- Use migrations as ordered, reviewed source of truth. Plan compatibility for rolling deploys: additive changes first, backfill safely, then remove old schema only after all callers have moved. Do not edit production schema out of band.
- Use transactions for a coherent set of writes that must succeed or fail together. Keep them bounded; pass transaction context explicitly through operations that must share it. Treat external API calls inside a database transaction cautiously because they hold locks without making the external effect atomic.
- Design indexes from real query predicates, joins, ordering, and measured plans. Account for write cost, uniqueness, and migration duration. Verify important query plans with representative data.
- Make pagination deterministic with a stable tie-breaker. Use keyset pagination for large/changing datasets where offset scans or shifting pages are a problem.

## MongoDB

- Model documents around access patterns and consistency boundaries. Embed data that is read/updated together and bounded; reference independently growing or shared entities. Avoid copying mutable facts without a reconciliation plan.
- Define schema validation and unique/compound indexes deliberately. Indexes affect writes and storage, so tie them to query patterns and validate with explain plans and representative data.
- Use atomic single-document updates where possible. Multi-document transactions have operational and performance costs; use them when invariants span documents and cannot be modeled more simply.
- Make migrations/backfills safe to resume, bounded, observable, and compatible with old and new application versions during rollout.

## Redis, caches, and queues

- Treat cache as a derived, disposable view of source data. Define key ownership, TTL, invalidation/update policy, serialization/versioning, and behavior during Redis failure. Prevent unbounded keys and sensitive data exposure.
- Use cache-aside or another explicit strategy; account for stale reads and stampedes where they matter. Do not make correctness depend on a cache unless Redis is intentionally the system of record for that data.
- Use a queue for asynchronous work that benefits from decoupling, buffering, retries, or independent scaling. Define payload versioning, ownership, delivery guarantees, visibility/lock behavior, and dead-letter or terminal-failure handling.
- Assume delivery can be duplicated. Make handlers idempotent using a durable operation key, unique constraint, inbox/processed record, or naturally idempotent update. Do not assume a queue provides exactly-once side effects.
- Retry transient failures with bounded attempts, exponential backoff, jitter, and a timeout/deadline. Do not retry permanent validation/authorization failures. Ensure shutdown stops intake and drains or safely releases in-flight work.
- When database state and event publication must agree, consider a transactional outbox and idempotent consumer; document how stuck events are detected and replayed.

## Workers

Keep job handlers focused on one capability, validate/version payloads, and make dependencies explicit. Set concurrency and resource limits based on downstream capacity. Capture correlation/job IDs in logs and metrics. Separate retryable errors from permanent failures and make retries safe. Test duplicate delivery, retry exhaustion, malformed payloads, and graceful shutdown when those conditions matter.

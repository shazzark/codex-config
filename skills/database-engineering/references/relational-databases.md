# PostgreSQL and MySQL

Use this guidance for relational engines. Check the deployed engine and version, SQL mode, schema, driver/ORM, and migration tool: behavior and syntax can vary.

## Model data and enforce integrity

- Use tables for entity types and rows for individual records. Give each table a stable primary key. Use foreign keys for relationships when they fit ownership and lifecycle; use unique, `NOT NULL`, `CHECK`, and other constraints to encode important invariants.
- Normalize facts that change independently or must remain consistent across uses. For example, store a customer's address once if orders need the current address; store an order-time address snapshot if the business needs the address as it was when the order was placed. That is a domain decision, not simply a normalization rule.
- Use join tables for many-to-many relationships when both sides are independently meaningful. Model optionality and deletion behavior deliberately; cascading deletion can be dangerous for retained or financial records.
- Prefer exact numeric types for money and quantities where rounding matters; define currency and rounding rules. Avoid relying on binary floating point for financial totals.
- Store timestamps with a clear timezone convention, typically UTC instants, and keep local display/timezone policy separate. Choose precision and null/default semantics deliberately.
- Use JSON columns only when flexible or sparse attributes justify them. Keep frequently queried or integrity-critical fields in typed columns when that makes constraints and indexes clearer.

## PostgreSQL and MySQL considerations

- Both engines support relational constraints and transactions, but supported SQL, DDL behavior, locking, isolation defaults, JSON features, index types, online DDL options, and replication differ by version and configuration. Verify engine-specific assumptions before relying on them.
- PostgreSQL offers a broad set of index types and transactional behavior for many schema changes; some large index operations have special concurrent forms and operational constraints. Check the exact operation, locks, and version before production rollout.
- MySQL behavior depends on storage engine (commonly InnoDB), version, collation, and SQL mode. Confirm transactional semantics and DDL locking/online behavior for the actual setup. Do not assume every engine/table has identical guarantees.
- Collation determines text comparison and sorting rules. Make case, accent, and uniqueness expectations explicit instead of assuming a universal string comparison.
- Keep SQL portable only when portability is a real requirement. Avoid hiding useful engine features behind lowest-common-denominator abstractions without a reason.

## Queries, indexes, and concurrency

- Write queries around the result the caller needs. Select required columns, filter early, bound result sizes, and avoid issuing one query per returned row (the N+1 pattern).
- Add indexes for observed filters, joins, uniqueness rules, and sort order. Composite index column order matters; align it with predicates and ordering. Avoid indexing every column, duplicating equivalent indexes, or assuming a low-selectivity index will help.
- Inspect execution plans using the engine's explain/profiling tools with representative data and parameters. Look for full scans on large tables, poor row estimates, expensive sorts, repeated lookups, and unexpected join strategies. An explain plan is evidence about a query and data distribution, not a guarantee for every production workload.
- Use deterministic ordering for pagination, including a unique tie-breaker. Use keyset/seek pagination when large offsets or changing result sets cause cost or instability.
- Use a transaction for a set of database changes that must commit together. Keep it short and avoid network calls inside it. Choose isolation based on the invariant; higher isolation can reduce anomalies but increase blocking or retries.
- Isolation levels control which concurrent changes a transaction can observe, but names and edge behavior vary by engine. Understand the chosen engine's guarantees for the anomaly you need to prevent instead of assuming the same level name behaves identically everywhere. Serializable execution may require retrying a transaction that cannot be serialized.
- Use optimistic concurrency when conflicts are uncommon: update with a version/expected-value condition and report a conflict if another writer changed the row. Use pessimistic locking (such as a row lock) when a short critical section must reserve existing rows; locks can block and deadlock, so acquire them in a consistent order and keep transactions short. Constraints and atomic conditional updates are often simpler and stronger.
- Treat deadlocks and serialization conflicts as retryable only when the entire operation can safely retry. Use bounded retries with backoff and idempotency; never repeat external side effects as part of a database retry.
- Connection pools protect the database from connection churn, but total pool capacity across all app instances must fit database limits. Set timeouts and observe pool wait, saturation, and connection errors.

## Prisma and Drizzle

- **Prisma:** Treat the Prisma schema and committed migration history as application artifacts. Review generated SQL and understand when `migrate` workflows differ from schema-push workflows; do not use a development convenience command as a production migration plan. Prisma's generated types do not replace database constraints or input validation.
- **Drizzle:** Keep schema definitions, generated/handwritten migrations, and runtime query code aligned with repository conventions. Review migration SQL, especially for large tables, data transformations, locks, and defaults. Use explicit transactions where a multi-statement change must be atomic.
- With either ORM, inspect emitted SQL for critical queries, use its transaction API without losing transaction context, and use parameterized/raw SQL where that is the clearest safe option. ORM APIs do not remove the need to understand the engine.

## Testing

Use an isolated database of the same engine and a compatible version for integration tests when feasible. Test constraints, joins, transaction rollback/commit, concurrency-sensitive behavior, and migrations where those are part of the contract. Unit tests with mocks cannot prove SQL validity, query plans, locking, or engine constraints.

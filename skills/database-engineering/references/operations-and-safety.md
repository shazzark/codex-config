# Database Operations and Safety

Use these practices across database engines. Match the depth to the system's risk, data sensitivity, workload, and recovery needs.

## Migrations and data lifecycle

- Inspect the live schema through approved read-only means, the migration history, and application query paths before planning a change. Migration files are not necessarily a perfect description of production state.
- Prefer small, observable, reversible where possible changes. For rolling deploys, use expand-and-contract: add compatible structures first, deploy code that can work with both shapes, backfill in bounded batches, move reads/writes, then remove obsolete structures in a later release.
- Make data migrations resumable and idempotent. Track progress, rate-limit load, handle partial failure, and verify counts/invariants. Avoid long table locks, huge unbounded transactions, and application downtime unless explicitly planned.
- A rollback script cannot always recover deleted or transformed data. Define whether recovery means rollback, forward-fix, restore, or reconciliation; take a verified backup/snapshot before risky change when appropriate.
- Seed only intended environments. Separate deterministic test fixtures from production reference data, and never copy sensitive production rows into tests or local machines without explicit approved sanitization.
- Define retention and deletion requirements, including indexes, replicas, caches, search systems, logs, backups, and legal holds. A row delete may not erase every copy immediately.

## Security and privacy

- Use separate database identities and least privilege per application/service/job where practical. Grant only required databases, schemas, tables, and operations; do not run applications as schema owner or administrator.
- Use parameterized statements or safe query-builder/driver APIs. Never concatenate untrusted input into SQL, filters, sort expressions, identifiers, or aggregation stages. Allowlist dynamic field names and sort options.
- Store credentials in an approved secret manager or protected runtime configuration. Rotate them, avoid printing connection strings, and keep them out of source, logs, issue examples, and test output.
- Use transport encryption and storage encryption according to the threat model and deployment. Encryption does not replace access control. Minimize sensitive data and define masking, retention, deletion, and audit rules.
- Restrict production access, require auditable change paths, and use read-only access for investigation when possible. Do not copy production records into prompts, screenshots, fixtures, or local environments.

## Testing and verification

- Use unit tests for application policy, but use a real isolated engine for database constraints, SQL/Mongo query behavior, transaction semantics, migrations, and driver integration when those behaviors matter.
- Run migrations forward and, where rollback is claimed, test rollback on disposable representative data. Check mixed-version compatibility and query behavior after the schema change.
- Test concurrency and retry paths for invariants that can race. Check both expected success and relevant conflict/failure cases.
- Verify query plans and latency against representative data and workload. Small fixtures can hide scans, skew, lock contention, or memory pressure.
- Never treat a passing test suite as proof of production migration safety, backup validity, capacity, failover, or restore readiness.

## Reliability and recovery

- Monitor connection/pool saturation, query latency/errors, locks/deadlocks, storage and growth, replication lag, backup age/results, and engine-specific health. Alert on actionable user impact and recovery risk.
- Health/readiness checks should be bounded and reflect whether the application can serve its required work without exposing database internals publicly. Avoid making every liveness probe depend on a database round trip.
- Plan capacity using data growth, index overhead, connection counts, working set, backups, replication, and peak concurrency. Load test the real bottleneck before increasing pool sizes or replicas.
- Replication can improve availability or read scale but introduces lag and failover behavior. Decide which reads require primary/strong consistency and what recovery-point and recovery-time objectives are acceptable.
- Backups are useful only if they can be restored. Automate backup verification where possible and perform restore/recovery exercises in an isolated environment. Document owner, retention, encryption, access, and recovery steps.
- Define behavior during database failure: timeouts, bounded retries with backoff, circuit/load shedding where appropriate, idempotency, and clear user-facing errors. Do not retry permanent errors or retry writes without understanding duplicate effects.

## Safe execution boundaries

- Treat production migrations, production writes/deletes, restores over live data, replica changes, and destructive DDL as protected operations. Do not perform them without explicit authorization and a reviewed plan.
- Before a destructive action, state the exact target, expected impact, backup/recovery method, and verification plan, and obtain explicit approval. Prefer a dry run and isolated rehearsal first.
- Redact secrets and sensitive records from diagnostics. Report only the minimum evidence needed to explain a database issue.

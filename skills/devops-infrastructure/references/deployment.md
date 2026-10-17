# Deployment and environment management

## Environment boundaries

Describe the role of development, CI, staging, and production. Use separate credentials and data, restrict access by environment, and keep production configuration explicit. Avoid copying production records to local/staging; use synthetic or sanitized data. Store non-secret configuration in the platform's supported configuration mechanism and credentials in a secret manager/protected secret store.

Choose managed PostgreSQL, MongoDB, or Redis when operational expertise, backup, patching, failover, and capacity requirements favor the provider and its cost/limits are acceptable. Self-host only when the team can operate, patch, monitor, secure, back up, and recover the service. Containerizing a database does not by itself make it production-ready. Database engine configuration, schema, and migration safety belong to `$database-engineering`.

For stateful services, make storage and recovery behavior explicit. Keep PostgreSQL data on durable storage and manage schema changes through the application's migration process, not only one-time container initialization. For MongoDB, match replica-set/sharding topology and storage durability to availability needs; a single local Compose container does not model production failover. For Redis, decide whether data is disposable cache data or durable session/queue/state data before choosing persistence, eviction, and backup behavior. Keep data services on private networks, require authenticated/TLS connections where supported by the environment, and verify the service-specific health signal. Exact engine settings remain with `$database-engineering` and `$security-engineering`.

## Release and traffic

Define release identity (version, tag, or commit), artifact provenance, target environment, and promotion/approval rules. For internet-facing services, configure a reverse proxy or hosting ingress to terminate HTTPS, preserve only trusted forwarded headers, route health traffic appropriately, and set body/time limits deliberately. TLS certificates, domains, proxies, and cloud load balancer behavior vary by provider; verify the target setup.

Use rolling, blue/green, or canary release only when availability needs and platform capability justify the additional complexity. Confirm old and new application versions can coexist during rollout. Health/readiness checks should prevent traffic going to instances that cannot serve requests; liveness checks should identify stuck processes without causing restart loops for a temporary downstream outage.

## Migration and rollout sequence

Coordinate migrations with deployment order:

1. Add schema in a backward-compatible form, if required.
2. Deploy code that can work with old and new schema during the overlap.
3. Backfill data in bounded, observable batches where needed.
4. Switch reads/writes after verifying the new path.
5. Remove old schema only after old application versions and consumers are gone.

This expand-and-contract pattern is one common strategy, not a mandatory sequence for every change. Ask `$database-engineering` to assess engine-specific locking, data safety, and rollback/forward repair. Database rollback is not automatically safe after writes.

## Verification and recovery

Before an approved release, check artifact identity, configuration presence without exposing values, migration status, service health, logs/metrics, and a small set of critical user-facing operations. Define who can stop or roll back the release and what signals trigger that decision. Use rollback only if schema/data compatibility allows; otherwise use a forward fix or traffic switch.

Recovery needs a valid backup, a known restore procedure, credentials/access, service dependencies, and a recovery-point objective (how much recent data may be lost) and recovery-time objective (how long service can be unavailable). Test restore in an isolated environment and record evidence. A successful backup job alone does not prove that data can be restored.

## Cloud environment choices

Compare container hosting, platform-as-a-service, virtual machines, and managed data services by required control, workload shape, team operations, scale, availability, data residency, network integration, and total cost. Include baseline, peak, storage, egress, observability, backup, and support costs. Avoid provider-specific service choices until requirements and constraints are known. Inspect current provider docs and plan limits before relying on exact behavior.

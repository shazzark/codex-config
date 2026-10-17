# Redis Caching

Use Redis when a shared low-latency cache solves a measured need across processes or when its data structures and operations fit the workload. Do not add it solely to cache one cheap local value. Decide whether each Redis dataset is disposable cache data or durable application state; persistence, eviction, and recovery settings differ.

## Keys, values, and data structures

- Use a consistent key format that identifies environment, application/domain, schema version, and object/tenant scope, for example `prod:catalog:v2:tenant-17:item-42`. Avoid putting secrets or personal data in key names.
- Include every dimension that affects the representation. Canonicalize filters/sort inputs before deriving a key; bound attacker-controlled key cardinality and length.
- Version serialized values or key namespaces when shape changes. Handle old/malformed payloads as cache misses rather than trusting unsafe deserialization.
- Use strings for simple serialized values/counters, hashes for field-oriented records, sets for unique membership, and sorted sets for ordered scores/rankings when their operations fit. These structures do not remove the need to define TTL and invalidation.
- Use lists, Streams, and Pub/Sub according to their delivery semantics, not as an implicit cache: Pub/Sub is transient; durable work requires explicit acknowledgements, retention, retries, and idempotency. See `backend-architecture` for queue guidance.

## TTL, memory, and eviction

- Set expiration as part of the write where practical to avoid leaving an immortal key after a partial operation. Avoid using one arbitrary TTL for unrelated data.
- Set a deliberate `maxmemory` budget and `maxmemory-policy` based on whether the dataset is disposable. An all-keys LRU/LFU policy may suit cache-only data; `noeviction` rejects writes at the memory limit. Confirm exact behavior and available policies for the deployed Redis version/service.
- Do not mix cache keys with sessions, locks, queues, or source-of-truth data under an eviction policy that can silently discard them. Separate instances or databases/configuration boundaries may be needed when durability requirements differ.
- Monitor memory fragmentation/usage, evictions, expirations, rejected writes, latency, connections, replication/failover, and persistence when enabled. Persistence and replication are not substitutes for a backup when Redis contains durable state.

## Invalidation and atomicity

- Prefer explicit key ownership and targeted invalidation. Broad wildcard scans or global flushes can be slow or destructive; do not use them as routine request-path invalidation.
- Use Redis atomic commands, transactions, or a reviewed Lua script when multiple cache changes must not interleave. Keep scripts bounded and remember that a Redis atomic operation cannot make an unrelated PostgreSQL/MongoDB write part of the same transaction.
- If invalidation must survive an application crash, persist the invalidation intent with the database write (for example, an outbox) and process it idempotently. TTL can bound damage but may not meet the freshness contract.
- Redis client-side caching/tracking is version- and client-dependent. If used, verify invalidation behavior on disconnect/reconnect and flush local copies when invalidation messages could have been missed.

## Clients, pools, and outage behavior

- Reuse a well-maintained Redis client and its supported connection pooling/multiplexing behavior; avoid creating a connection per request. Bound connection count and command duration, and review reconnect/backoff behavior for the installed client and deployment.
- Put timeouts and cancellation around cache calls so an unhealthy cache does not indefinitely block requests. Avoid unbounded retry loops.
- On failure, fall back to the source only when doing so is correct and the source can handle the extra load. Use concurrency limits, rate shedding, stale-if-safe behavior, or a controlled error response to prevent a Redis outage from stampeding the database.
- Do not let missing cache state grant permission or revive revoked access. Critical authorization and write invariants belong in an authoritative store or need a deliberately consistent design.

## Security and operations

Keep Redis on restricted networks, use authentication and narrow ACLs, protect credentials, and use TLS where required by the network boundary. Limit command/key access and exposure to admin/monitoring commands. Assess backup, failover, encryption, and data retention according to whether the instance holds disposable cache data or sensitive/durable state.

Official references: [Redis key eviction](https://redis.io/docs/latest/develop/reference/eviction/), [Redis client-side caching](https://redis.io/docs/latest/develop/clients/client-side-caching/), and the repository's [database-engineering Redis guidance](../../database-engineering/references/redis.md).

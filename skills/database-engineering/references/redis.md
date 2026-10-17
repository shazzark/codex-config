# Redis

Redis can serve as a cache, coordination store, queue substrate, or primary data store when the durability and data model fit. Decide which role each key has; operational settings and acceptable data loss depend on that role.

## Caching and invalidation

- Treat cached values as derived data unless Redis is intentionally the source of truth. Write down the cache key, owner, value format/version, TTL, invalidation/update rule, and behavior when Redis is unavailable.
- Cache-aside is a common pattern: read the cache, load from the source database on a miss, then populate the cache. On writes, update the source of truth and invalidate or refresh related cache keys. The order and retry behavior affect stale-data windows.
- TTL bounds how long an untouched key can remain, but does not by itself guarantee fresh reads. Use explicit invalidation, versioned keys, or a stronger design when stale values could violate a business invariant.
- Prevent cache stampedes with request coalescing, bounded refresh, or stale-while-revalidate only when load measurements justify the added behavior. Bound key cardinality and value size.
- Cache failures should normally fall back to the source database when safe, with timeouts and load protection so a Redis outage does not overwhelm that database.

## Locks and coordination

- A Redis lock is a time-limited coordination aid, not proof that a client still owns a critical section. A paused client can resume after its lease expires while another client has acquired the lock.
- Use unique lock tokens and atomic conditional release. Set leases and renewal behavior deliberately. For correctness-sensitive writes, prefer database constraints/transactions or use fencing tokens that the protected resource checks; a lock alone may not prevent stale work.
- Avoid distributed locks when a unique constraint, atomic database update, partitioned queue, or single-writer design is simpler and stronger.

## Queues and pub/sub

- Redis Pub/Sub is ephemeral broadcast: subscribers that are disconnected do not receive retained messages. Use it for transient notifications when losing a message is acceptable, not as a durable job queue.
- For work that must be retained and acknowledged, choose an appropriate queue mechanism (such as Streams or a queue library) and define persistence, acknowledgement, visibility/reclaim, retry, dead-letter, and consumer-group behavior. These guarantees depend on configuration and implementation.
- Assume a job may be delivered more than once. Make handlers idempotent and record enough information to detect stuck, failed, and replayed work.

## Persistence, eviction, security, and operations

- Configure persistence (if any), replication, failover, and backup according to acceptable recovery point and recovery time. Replication is not a backup; accidental deletion or corruption can replicate too.
- Choose `maxmemory` and eviction policy based on the role. Eviction may be acceptable for cache keys and unacceptable for sessions, queues, locks, or primary data. Monitor memory, evictions, latency, rejected writes, replication health, and persistence status.
- Restrict network exposure, use ACLs/least privilege, protect credentials, and use encrypted transport where supported/required. Avoid storing sensitive records in cache unless retention and access controls are appropriate.
- Test expiry, invalidation, Redis outage, eviction, and duplicate job handling where these affect correctness. A local in-memory fake will not prove Redis atomicity, expiry, persistence, or failover behavior.

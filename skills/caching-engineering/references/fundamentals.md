# Caching Fundamentals

## Decide whether to cache

Caching trades storage, invalidation work, and extra failure modes for fewer or faster reads. First determine whether the bottleneck is the database, network, serialization, rendering, or repeated computation. A better query, index, batching, pagination, or simpler rendering may solve it without another cache.

Caching is often harmful when data changes frequently, reads are rare, results are cheap to compute, freshness must be immediate, keys have high cardinality, memory is constrained, or the cache costs more to operate than the work it avoids. Do not cache merely because Redis or a framework offers a feature.

For every cache, record:

- **Value and source:** exactly what representation is stored and which system remains authoritative.
- **Audience and key:** who may reuse it and every input that changes the result, such as tenant, user, permissions, locale, query/filter, or schema version.
- **Freshness:** acceptable staleness, TTL/expiration, and whether requests may use stale data while refresh runs.
- **Change handling:** how writes invalidate or replace affected values, including partial failure and retries.
- **Failure behavior:** whether a miss, timeout, cache outage, or malformed value falls back, fails closed, or serves known-stale data.
- **Capacity:** expected key count, value size, churn, memory budget, and eviction policy.

## Reads and writes

- **Cache-aside (lazy loading):** the application checks the cache, reads the source on a miss, then stores the result with a bounded lifetime. It is simple and only fills requested keys, but concurrent misses can duplicate work and a write can leave stale entries.
- **Read-through:** a cache component loads the source on a miss. This can centralize the policy but introduces a dependency abstraction; use it only when its shared behavior is useful.
- **Write-through:** a write updates the source and cache as one logical operation. Decide ordering, error handling, and what happens when one update succeeds and the other fails. Separate systems rarely provide one atomic transaction.
- **Write-behind (write-back):** the cache acknowledges a write before the durable source is updated. Use only when delayed persistence and potential data loss are acceptable and a durable queue/replay/reconciliation design exists. It is not a default for business records.

For database-backed cache-aside, usually commit the authoritative write first, then invalidate or update the cache. If the process can fail between commit and invalidation, use a bounded TTL as a safety limit or a durable outbox/change stream to retry invalidation. An outbox couples an event record to the database transaction; it still requires an idempotent consumer and monitoring. Versioned keys can make old values unreachable but require a reliable version source and cleanup strategy.

Invalidation ordering matters. Deleting before a database write can allow a concurrent reader to refill the old value. Updating the database and then deleting can leave a short stale window or lose invalidation on process failure. There is also a stale-refill race: a reader can load the old source value before the write, pause, then populate the cache after the writer has committed and deleted the key. An outbox makes invalidation intent durable but does not, by itself, stop that in-flight stale fill. For bounded staleness, define and test the remaining window. For strict freshness, bypass the cache for that read or use a race-safe version/fencing or coordination protocol whose ordering is verified against the actual database/cache timing. Do not claim cross-system atomicity where none exists.

## Expiration, eviction, and staleness

- A TTL (time to live) places an upper bound on how long an untouched entry remains, subject to the store and clocks. It does not make a value fresh during that interval and is not a substitute for an invalidation rule when updates must appear sooner.
- Use shorter TTLs for rapidly changing or riskier data; longer TTLs only when the content and reuse pattern justify them. Add small random jitter to large groups of similar expirations when synchronized expiry could create load spikes.
- Eviction removes entries to stay within a memory budget. Treat an evicted cache entry as a miss, never as a data-loss event for the source of truth. Track evictions and memory pressure.
- Stale-while-revalidate can return an older value while refreshing asynchronously. Define the maximum stale age and use it only where serving old data is safe. Do not apply it to authorization, prices/inventory at checkout, or other operations whose invariant forbids stale reads.
- Negative caching stores an absence result briefly to avoid repeated expensive misses. Keep the TTL short, invalidate it when the object is created, and avoid exposing account/resource existence through response differences.
- Cache warming preloads likely-needed entries. Warm only a bounded, justified set and avoid competing with live traffic during startup or recovery. Cold-cache behavior still needs safe load limits.

## Stampedes and concurrency

A cache stampede (or thundering herd) occurs when many requests miss or expire the same key at once and all repeat the expensive source work. Start with request coalescing/single-flight inside one process, TTL jitter, bounded source concurrency, or serving acceptable stale data during one refresh. These approaches may not coordinate different app instances.

Use a distributed lock only when measurements justify it and its lease, ownership token, timeout, and release behavior are correct. A paused lock holder may continue after its lease expires; a lock alone does not protect a correctness-critical write. Prefer source-level atomic updates, uniqueness constraints, or idempotent work for invariants.

## Measure the trade-off

Compare representative traffic with and without the cache. Measure hit and miss counts, cache lookup time, end-to-end latency distributions, database query rate/latency/load, refresh/invalidation lag, stale serves, errors/timeouts, evictions, memory, and network bytes. Hit rate alone can look good while response latency or correctness gets worse. Include cold-start and cache-outage behavior.

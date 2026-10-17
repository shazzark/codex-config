# Cache Operations and Verification

## Observability

Collect metrics that explain usefulness, cost, correctness, and failure:

- hit, miss, bypass, negative-hit, refresh, and invalidation counts;
- cache lookup and fill latency, application response latency distributions, and source query count/latency/load;
- stale responses and invalidation lag when measurable;
- timeout, connection, serialization, and cache errors;
- memory, key/value size, key cardinality, TTL distribution, evictions, and Redis rejected writes;
- stampede indicators such as concurrent fills and source requests per cache key.

Use bounded-cardinality labels; do not put user IDs, raw cache keys, tokens, or private values in metrics. Logs and traces can record a safe key class or cache outcome without exposing data. A high hit rate is not enough if responses are wrong, latency increases, or the source remains overloaded.

## Performance verification

1. Measure the existing behavior with representative traffic/data and a repeatable workload. Record baseline latency distributions, source query rate/load, and resource usage.
2. Add the smallest cache design and retain a way to compare or bypass it safely during testing.
3. Repeat under comparable warm and cold cache conditions. Include cache lookup overhead, fills, invalidation, and realistic key churn.
4. Compare end-to-end p50/p95/p99 latency, source load, error rate, cache memory/eviction, and correctness. Report sample size, environment, and workload limits.
5. Keep the cache only if the measured benefit outweighs operational cost and the freshness contract passes.

Do not claim a speedup from a design review, cache hit ratio alone, or a benchmark that omits misses, writes, concurrency, or the source workload.

## Correctness and regression tests

Choose tests for the cache boundary and consistency contract:

- hit and miss return the same authorized representation;
- expiration and negative-entry behavior;
- create/update/delete invalidation and version/serialization changes;
- tenant, user, role, locale, filter, and permission isolation;
- write/invalidation races and simultaneous fills for hot keys;
- stale-while-revalidate bounds and refresh failure;
- Redis timeout/outage, malformed values, reconnect, eviction, and source fallback/load shedding;
- HTTP headers, ETag/`304`, CDN shared-cache isolation, and private-response no-store/private behavior;
- Next.js cache/revalidation and TanStack Query invalidation/hydration on the actual installed versions.

Unit tests can verify key construction and policy. Integration tests with the real cache and database are needed for Redis atomicity, TTL/eviction behavior, database-write/invalidation ordering, and client/framework semantics. Use isolated test infrastructure. Do not use production data or claim distributed behavior from an in-memory fake.

## Capacity and recovery

Estimate memory from key count, average serialized value size, Redis object overhead, replication/buffer use, and expected growth; then validate under realistic load. Bound entry size, key cardinality, fill concurrency, connection counts, and retries. Plan warm-up and cold-start load on deployment and recovery. Set alerts for the behavior that affects users or source capacity, not just the presence of a Redis process.

Treat disposable-cache recovery differently from durable Redis state. Define whether restart/failover should produce an empty cache, tolerate stale data, or restore persisted values. Verify the plan in an isolated environment where recovery requirements justify it.

# Caching Engineering Scenario Reviews

These five checks are design exercises against the skill's instructions. They are not application runtime tests or performance measurements. No application, Redis server, database, CDN, or live workload was exercised.

## 1. Redis for a frequently requested REST endpoint

**Scenario:** A read-heavy Express endpoint repeatedly loads a popular catalog summary from PostgreSQL.

**Review result:** First inspect request/query rate, query plan and latency, result change frequency, and response audience. If measured source cost justifies it, use cache-aside with a versioned key, bounded value size, explicit TTL with jitter, and write-triggered invalidation. Coalesce concurrent fills for hot keys and retain authorization/tenant checks. Instrument hits, misses, fill latency, database load, and evictions.

**Assessment:** Proportionate shared-cache choice only after evidence; design exercise only. No Redis integration test or benchmark ran, so no speedup is claimed.

## 2. Invalidation after PostgreSQL or MongoDB updates

**Scenario:** A successful update must appear promptly in cached product details.

**Review result:** The database remains authoritative. Commit the data change, then invalidate/update affected cache keys. If a process crash must not lose invalidation, write an outbox/change event in the same database transaction and process it idempotently; this makes invalidation durable but does not alone prevent an in-flight reader from repopulating an old value after deletion. For bounded staleness, state and test the remaining window. For strict freshness, bypass the cache for that read or use a race-safe version/fencing or coordination protocol validated against actual timing. For MongoDB, use an atomic document update for the source invariant where possible; do not imply a Redis operation participates in its transaction.

**Assessment:** Separates database atomicity from cache consistency and identifies failure races; design exercise only. No database or cache transaction was executed.

## 3. Next.js and HTTP public/private content

**Scenario:** Public product pages are CDN-cacheable, while account pages use cookie-authenticated data.

**Review result:** Public pages can use a shared-cache policy only when the CDN key fully represents the response. Account responses use a private/no-store policy as required, and do not enter a shared Next.js or CDN cache. Inspect the installed Next.js version and cache model; apply explicit revalidation after writes. A request-scoped TanStack `QueryClient` prevents server-rendered user data crossing requests; client query keys include the user's tenant/context.

**Assessment:** Keeps cache audiences separate across HTTP/CDN, server, and client caches; design exercise only. No browser, Next.js, or CDN test ran.

## 4. Multi-instance API, Redis outage, and stampede

**Scenario:** Several Fastify instances share Redis, which becomes unavailable as a popular key expires.

**Review result:** Bound Redis timeouts/retries, coalesce fills per instance, jitter expiry, and limit database fallback concurrency. Serve stale data during refresh only if the content's freshness contract allows it. Track source load, fill concurrency, cache errors, stale serving, and invalidation lag. Test multi-instance races and outage recovery with the real Redis/client topology before claiming distributed behavior.

**Assessment:** Addresses herd risk and avoids converting a cache outage into database overload; design exercise only. No outage or concurrency test was executed.

## 5. Small application without a caching need

**Scenario:** A small internal application has low traffic and a fast indexed query; no user-visible latency or database capacity issue has been measured.

**Review result:** Do not add Redis or an application result cache. Keep the query/database path straightforward, retain relevant baseline observability, and revisit only if measurements show a problem. If HTTP validation or existing framework caching has no meaningful benefit either, use no cache.

**Assessment:** Follows the rule that caching is optional and avoids unjustified operational complexity; design exercise only. No performance test was run.

## Overall result and limitations

All five cases were reviewed for source-of-truth clarity, module placement, isolation, invalidation/failure behavior, complexity, and evidence. The outcomes are proposed designs, not observed application behavior. No performance improvement is claimed. Exact TTLs, key layouts, policies, Next.js behavior, Redis eviction, and consistency guarantees require the target workload, installed versions, deployment topology, and executable tests.

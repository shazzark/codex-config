---
name: caching-engineering
description: Design, implement, optimize, troubleshoot, and verify application caches. Use when caching behavior, freshness, invalidation, or cache operations materially affect a system.
---

# Caching Engineering

Design caching only when measured performance, load, latency, or availability needs justify the extra state and operations. A cache is a faster copy or computed result; the source of truth remains the authoritative system unless the application deliberately assigns durable ownership elsewhere. Do not add a cache, Redis, or a new abstraction by default.

The principles apply across languages. Version 1 has focused guidance for Node.js, TypeScript, Express, Fastify, Next.js, PostgreSQL, MongoDB, Redis, HTTP caches, and TanStack Query. Read [caching fundamentals](references/fundamentals.md) for strategy and freshness, [Redis](references/redis.md) for Redis behavior, [backend, database, and HTTP integration](references/backend-and-http.md) for common application boundaries, and [operations and verification](references/operations-and-verification.md) for metrics, failure handling, and tests.

## Workflow

1. Inspect the existing request/data flow, source of truth, query paths, write paths, framework/version, caches, authorization checks, metrics, and tests.
2. Establish the problem with representative measurements: which operation is slow or costly, how often it runs, who needs the result, how often its source changes, and what freshness the use case requires. If there is no concrete problem, do not introduce caching.
3. Choose the simplest cache layer and pattern that fits: browser/CDN/HTTP, framework/client state, in-process, or shared cache. State what is cached, its owner/key, lifetime, invalidation rule, and miss/error behavior.
4. Preserve source-of-truth and transaction boundaries. Define acceptable staleness and consistency by operation; never use cached values to decide critical permissions, balances, inventory, or other invariants unless the design explicitly guarantees the required freshness.
5. Preserve user and tenant isolation. Include every relevant representation and authorization dimension in keys, or avoid shared caching for private data. Authorization must still be enforced on every request.
6. Add correctness tests for hits, misses, writes/invalidation, concurrency, private-data isolation, and cache failure as relevant. Benchmark before and after with comparable workloads.
7. Review memory, eviction, connection limits, observability, deployment behavior, and rollback. Report measured results separately from expected benefits.

## Engineering rules

- Identify and name the authoritative source before caching a copy.
- Explain why caching is needed and select the smallest suitable strategy. Prefer existing database, framework, browser, or CDN features when they solve the measured problem.
- Define TTL and invalidation/update behavior explicitly. TTL limits retention; it does not by itself guarantee freshness.
- Preserve authorization and tenant boundaries across cache keys, HTTP/CDN policy, client state, and server-side rendering.
- Handle cache timeout, outage, eviction, and malformed/stale values safely. A fallback to the database needs load protection so a cache outage does not overload the source.
- Do not cache security-sensitive or user-specific data in a shared cache unless its isolation and retention are intentional and verified.
- Avoid Redis when an in-process cache, HTTP validator, database optimization, or no cache is sufficient.
- Measure representative latency, source load, cache overhead, memory, and correctness before claiming an improvement.
- Include targeted correctness and regression tests. Distinguish tested improvements from expected or unmeasured ones.
- Explain the decision and freshness/performance trade-offs in plain English.

## Work with related skills

- Use `backend-architecture` for module boundaries and application structure. This skill decides where caching belongs and what behavior its boundary must provide.
- Use `database-engineering` for query plans, indexes, data ownership, transactions, and persistence behavior. Optimize the source query before adding a result cache when that may be simpler.
- Use `security-engineering` for sensitive data, tenant isolation, cache poisoning, and broader threat review.
- Use `engineering-workflow` for phased implementation, repository verification, documentation, and Git checkpoints.
- Use `debugging-workflow` when investigating an observed cache defect, stale response, outage, or performance regression; use measurements and reproduction before changing the strategy.
- Use `plain-english-explainer` for detailed teaching; keep ordinary implementation summaries concise and explain what changed and what was actually verified.

## Invocation examples

- Use `$caching-engineering` to assess whether this slow Express endpoint needs Redis caching; compare the query cost and request rate first.
- Use `$caching-engineering` with `$database-engineering` to define cache invalidation after this PostgreSQL write.
- Review public and private response caching in this Next.js app, including CDN headers and user isolation.
- Use `$debugging-workflow` to investigate this stale API response, then use `$caching-engineering` to verify the invalidation path.

## Scope and limitations

Cache behavior depends on client, framework, database, Redis, CDN, and deployment versions. Inspect the installed versions and consult current authoritative documentation before using version-specific APIs. Design reviews and cache metrics do not prove correctness or performance by themselves; verify the actual workload and consistency requirements.

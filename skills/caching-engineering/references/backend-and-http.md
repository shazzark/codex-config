# Backend, Database, Browser, and Framework Caching

## Application and database boundaries

- Put application cache policy at the narrowest shared point that serves the measured use case. A service/use-case or data-access function often knows the source and invalidation events; a route handler may be enough for a one-off HTTP response. Do not add a cache repository/interface layer unless it clarifies ownership, policy, testing, or substitution.
- Keep cache keys, TTLs, serialization, and invalidation near the feature that owns the data or in an intentional shared adapter. Avoid unrelated global cache helpers that hide key ownership.
- **PostgreSQL:** First inspect `EXPLAIN`/query metrics, indexes, pool behavior, and repeated query patterns with `database-engineering`. PostgreSQL already has buffer and plan caches; application result caching is a separate copy with separate freshness obligations. Do not cache query results as a substitute for an appropriate plan or schema.
- **MongoDB:** Profile query shape, indexes, projections, and document access patterns first. Cache only results with meaningful reuse and a defined invalidation/version rule; result caches can become stale independently of MongoDB's own behavior.
- On writes, commit the database transaction before invalidating derived cache state unless a different explicit protocol is necessary. If write and invalidation must survive crashes, coordinate via an outbox/change stream or reconciliation and make consumers idempotent. Do not assume Redis and a database share an atomic transaction.
- For critical decisions (authorization, account state, financial/inventory constraints), query or verify authoritative state at the point required by the invariant. A cached display value may be acceptable where a cached decision is not.

## Express and Fastify

- Decide whether the cache stores domain/query data or an HTTP response. Data caching can preserve per-request authorization and response shaping; response caching is safe only when the representation and cache audience are explicit.
- For Express and Fastify, put cache policy where all intended requests pass, keep auth/authorization checks before private data is served, and use framework/client APIs compatible with the installed version. Set timeouts and fail behavior for cache calls.
- Add response caching only to suitable `GET`/`HEAD` representations; never let a cache key omit query parameters, language, tenant, authorization, or other representation inputs. Be careful with compression/content negotiation and `Vary` behavior.
- Do not let a cache hit bypass server authorization, freshness requirements, validation, or required audit effects.

## HTTP, browser, proxy, and CDN caches

- Set cache policy deliberately. `Cache-Control: no-store` means do not store; `no-cache` permits storage but requires validation before reuse. `private` limits storage to a private cache such as a browser; `public` and `s-maxage` can permit shared-cache reuse. Do not use public/shared caching for personalized responses unless the audience is safely partitioned and the product explicitly intends it.
- For sensitive or account-specific responses, choose a conservative policy (often `private` or `no-store` depending on sensitivity and requirements). Ensure CDN rules do not override origin headers or cache authenticated responses across users.
- ETags and `Last-Modified` validators support conditional requests. A matching validator can return `304 Not Modified` and avoid retransmitting the body, but the origin may still do work to determine whether the representation changed. Make validators represent the correct version and audience.
- Where supported by the browser/CDN and appropriate for the content, `stale-while-revalidate` can serve a bounded stale response while refreshing in the background; `stale-if-error` can permit bounded stale service during origin failure. Define the maximum stale window and do not use these allowances where the business rule requires current data.
- Use long-lived caching for content-addressed/versioned static assets when filenames change with content. Give HTML and mutable API data policies that match their update and revalidation contract.
- Review CDN cache key, query-string behavior, cookies, authorization headers, `Vary`, purge/invalidation, stale serving, and bypass rules. Test a private request followed by a different user's request against the actual shared-cache configuration.

## Next.js and TanStack Query

- Inspect the installed Next.js version and whether Cache Components are enabled before recommending APIs. Next.js 16's `cacheComponents` model uses explicit `use cache`, `cacheLife`, and tag/path revalidation; apps using the previous model have different caching and revalidation behavior. Do not mix examples across models without checking the target version.
- Cache only public or deliberately audience-scoped data. Keep request-specific/user-specific values out of shared server caches unless the framework supports a correctly isolated cache key and the authorization model is verified. Revalidate after mutations at the owning data boundary and account for stale windows.
- In React, TanStack Query is client-side server-state caching, separate from HTTP, Next.js server, and CDN caches. Define stable query keys including every data/tenant dimension, choose `staleTime` for freshness and `gcTime` for unused-data retention according to installed version, and invalidate/update affected queries after mutations. Stale data can remain visible while a background refetch runs.
- For server rendering/hydration, use a request-scoped `QueryClient`; never share user-specific query state in a process-global client. Avoid unbounded server-side cache memory and verify hydration/refetch behavior for the installed version.

## References

- [Next.js revalidating data](https://nextjs.org/docs/app/getting-started/revalidating) and [caching without Cache Components](https://nextjs.org/docs/app/guides/caching-without-cache-components)
- [Next.js `cacheComponents`](https://nextjs.org/docs/app/api-reference/config/next-config-js/cacheComponents)
- [TanStack Query important defaults](https://tanstack.com/query/latest/docs/framework/react/guides/important-defaults)
- [TanStack Query server rendering](https://tanstack.com/query/latest/docs/framework/react/guides/ssr)
- [HTTP caching guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching)
- [HTTP conditional requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Conditional_requests)
- [HTTP `Cache-Control` reference](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cache-Control)

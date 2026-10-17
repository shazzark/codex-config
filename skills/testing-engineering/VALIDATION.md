# Testing-engineering scenario review

These six checks are design exercises evaluating the skill's guidance. No sample application or test suite was supplied, so no executable test, database connection, browser run, Playwright session, load test, or CI workflow was run.

| Scenario | Appropriate guidance and review result | Check type |
|---|---|---|
| Unit test a TypeScript utility library | Inspect current runner/types/config; test public behavior, edge and invalid inputs, and errors; use small fixtures and focused collaborators; run typecheck and unit tests. Keep coverage tied to meaningful behavior. | Design exercise; no source package or test command executed. |
| Fastify API with PostgreSQL | Use `app.inject()` for HTTP lifecycle and a real isolated PostgreSQL database for SQL, constraints, transactions, and migration behavior. Test valid/invalid requests and rollback; keep database fixtures isolated and cleanup safe. | Design exercise; no API or database started. |
| Authentication and authorization workflows | Verify successful login/session/token paths plus denied roles, ownership, tenant boundaries, expired/revoked credentials, recovery abuse, and CSRF as applicable. Use synthetic users and provider test mode; mocks do not prove cookie/protocol integration. | Design exercise; no provider/browser/security test run. |
| Next.js E2E with Playwright | Cover a few critical user journeys using accessible locators and visible outcomes. Keep test data isolated, avoid fixed sleeps, and add representative responsive/accessibility checks. Separate screenshots from functional assertions. | Design exercise; no Next.js server or browser session. |
| Cache invalidation and concurrency | Test hit/miss/expiry and write invalidation with an isolated real Redis when behavior depends on Redis; exercise concurrent readers/writers, duplicate work, and failure fallback as needed. Benchmark separately before claiming performance. | Design exercise; no Redis or concurrency test. |
| CI strategy for a multi-service app | Run quick checks early; provision isolated service dependencies for integration tests; gate costly E2E/performance checks by risk; isolate parallel databases/ports; protect secrets and restrict workflow permissions; report each job status. | Design exercise; no GitHub Actions run or service stack. |

## Findings and limitations

The scenarios prioritize a small set of behavior-driven tests while retaining real integration checks where mocks cannot establish framework, database, protocol, or concurrency behavior. They keep architecture, database, security, cache, UI, and operations decisions with their respective skills. This review does not demonstrate that an implementation passes tests or that a test plan covers all requirements; actual code, environments, and executed results are still required.

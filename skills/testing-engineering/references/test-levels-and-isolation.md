# Test levels and isolation

## Select the test boundary

- **Unit:** a focused function or module with controlled collaborators. Check meaningful inputs, outputs, state changes, and failures. Keep the boundary small enough that a failure points to a useful cause.
- **Integration:** two or more real parts working together, such as an API handler with a database adapter, a package with its filesystem boundary, or a React component with DOM behavior.
- **Contract:** compatibility across independently changed boundaries, such as an API provider and consumer or a serialized message schema. A contract test checks selected promises; it does not replace end-to-end checks for user journeys.
- **End-to-end (E2E):** a critical user flow through a running application and realistic dependencies. Keep E2E focused because it is slower and more environment-sensitive.

Choose test depth by risk, change frequency, and cost. Do not treat a test pyramid or fixed percentage as a mandatory rule. Many stable business decisions can be tested cheaply at unit level, but persistence, networking, browser behavior, and cross-service contracts need some real-boundary checks.

## Doubles, fixtures, and deterministic behavior

A **stub** supplies a controlled response, a **fake** provides a simpler working substitute, a **mock** verifies an interaction, and a **spy** records calls to a real or wrapped function. Use each deliberately:

- Mock slow, unavailable, nondeterministic, or expensive external systems for focused unit behavior.
- Use the real implementation at least at suitable integration boundaries when protocol, serialization, query, transaction, or framework behavior matters.
- Avoid mocking the same code path the test is supposed to verify. A passing mock test cannot establish that the real server, database, or provider accepts the operation.
- Use small fixture builders/factories with meaningful defaults. Keep important values explicit in the test and avoid giant shared fixtures with hidden behavior.
- Reset state or create unique test records; cleanup must not erase data outside the test's isolated namespace/database.
- Control clocks, random IDs, retries, and async completion with supported fake timers or injected sources when possible. Restore global fakes after each test.

Tests should not rely on a prior test, order, shared mutable state, external internet, developer-specific files, or production credentials. Use bounded timeouts. If cleanup can fail, surface that failure rather than silently leaving state behind.

## TypeScript and JavaScript tools

Inspect `package.json`, lockfile, runtime, and existing scripts first. Use the project's configured runner—often Vitest or Jest for TypeScript/JavaScript, or Node.js's built-in test runner when its features fit. Do not add a second runner by default.

Use typechecking as a separate quality check where the test runner does not typecheck sources. Keep tests typed enough to catch invalid fixtures and API use, but avoid overengineering test types. For asynchronous tests, await the operation under test and all relevant cleanup; do not allow a test to finish while promises remain in flight.

Official references: [Vitest](https://vitest.dev/guide/), [Jest](https://jestjs.io/docs/getting-started), and [Node.js test runner](https://nodejs.org/api/test.html). Check the version installed in the repository before relying on specific APIs.

---
name: testing-engineering
description: Select, design, implement, execute, debug, and maintain meaningful software tests across application layers and platforms.
---

# Testing Engineering

Use this skill when deciding what to test, adding or improving tests, investigating test failures, or building a test strategy. It applies to frontend, backend, APIs, databases, distributed services, and mobile applications. Principles are language-independent; version 1 provides focused guidance for TypeScript, JavaScript, Node.js, React, Next.js, Express, Fastify, PostgreSQL, MongoDB, Redis, and React Native.

## Workflow

1. Inspect existing tests, test scripts, configuration, CI, runtime/tool versions, fixtures, and test environment before changing them.
2. State the behavior or contract under test, its users and risks, relevant boundaries, and what evidence would establish the result.
3. Choose the smallest set of meaningful tests at the lowest useful boundary. Add broader integration or end-to-end coverage where a real boundary or user journey matters.
4. Prefer observable behavior over private implementation details. Use real dependencies when their integration behavior matters and an isolated environment is available; use doubles when they make a focused unit test clearer or control an external failure.
5. Keep test data, time, randomness, network, state, and cleanup controlled. Tests should be repeatable and independent of execution order.
6. Run the relevant tests when execution is available. If something fails, investigate the cause; do not weaken assertions, remove coverage, or add retries just to get a green result.
7. Report separately which tests were written, which were executed, which passed/failed, and which were not run. Explain what the evidence does and does not establish in plain English.

## Engineering rules

- Identify the behavior and risk being tested; prioritize correctness and risk coverage over test count.
- Inspect the current test conventions before adding a framework, helper, or test layer.
- Prefer behavior-focused tests. Avoid excessive mocking and tests that only verify mock calls or mirror implementation details.
- Use real databases, caches, HTTP handlers, or providers for integration behavior when an isolated environment is available and safe.
- Keep tests deterministic, isolated, order-independent, and explicit about timeouts and cleanup.
- Include relevant negative paths, boundaries, regression cases, and security-sensitive denials.
- Preserve established behavior unless a change is approved. A test failure is evidence to investigate, not an invitation to relax the check.
- Never use production databases, real customer records, or destructive live-system tests without explicit authorization. Prefer isolated ephemeral or dedicated test resources.
- Run tests rather than only writing them when execution is available. Never claim results that were not observed.
- Report coverage honestly. A coverage percentage shows which code ran, not whether assertions prove correctness.
- Explain test-level and tool choices in plain English.

## References

Read only references relevant to the task:

- [Test levels and isolation](references/test-levels-and-isolation.md) for unit/integration scope, doubles, fixtures, determinism, and TypeScript/JavaScript tools.
- [Services, contracts, and data](references/services-and-data.md) for HTTP APIs, contract checks, PostgreSQL, MongoDB, Redis, and transaction/concurrency testing.
- [UI and end-to-end](references/ui-and-end-to-end.md) for React, Next.js, Playwright, accessibility, visual checks, and React Native/Expo.
- [Reliability and CI](references/reliability-and-ci.md) for load/failure testing, flakiness, coverage, GitHub Actions, and release confidence.
- [Scenario review](VALIDATION.md) for the six design exercises and what was or was not executed.

## Work with related skills

- Use `$engineering-workflow` for substantial implementation phases, repository checks, documentation, and Git checkpoints; this skill determines test selection and evidence.
- Use `$debugging-workflow` for systematic root-cause analysis of a failing test or behavior; do not change assertions before understanding the failure.
- Use `$language-engineering` for language-level correctness and testability.
- Use `$backend-architecture` for backend boundaries and API/module design; test the contracts that matter at those boundaries.
- Use `$database-engineering` for data models, query semantics, transaction guarantees, migrations, and database operations; this skill guides how to verify them.
- Use `$security-engineering` for threat modeling and security requirements; add negative-path security regression tests without claiming a test suite proves security.
- Use `$authentication-engineering` for identity/session/token and authorization behavior that authentication tests should protect.
- Use `$caching-engineering` for cache freshness, invalidation, failure, and performance expectations.
- Use `$design-system-engineering` for component behavior, accessibility, responsive layouts, and visual validation.
- Use `$devops-infrastructure` for CI orchestration, environment setup, deployment, observability, and recovery; this skill owns test strategy within that pipeline.
- Use `$plain-english-explainer` for detailed teaching; this skill asks that test decisions and results be understandable by default.

These skills retain domain ownership. Do not copy their entire architecture, security, database, or operations guidance here.

## Invocation examples

- Use `$testing-engineering` to choose a small but meaningful test set for this TypeScript utility library.
- Add Fastify integration tests against an isolated PostgreSQL database; verify constraints, rollback, and invalid requests.
- Review these Playwright tests for realistic user behavior, accessibility, and brittle selectors.
- Use `$debugging-workflow` to investigate this flaky test, then use `$testing-engineering` to prevent the underlying race from returning.
- Plan GitHub Actions test stages for this multi-service application without running production data tests.

## Scope and limitations

Initial detailed examples focus on the listed TypeScript/JavaScript ecosystem. Framework and runner APIs change; inspect installed versions and consult current official docs. A green suite covers only its tested behaviors and environment. Design reviews, mocks, coverage percentages, and local test passes are not proof of production correctness, security, performance, or recoverability.

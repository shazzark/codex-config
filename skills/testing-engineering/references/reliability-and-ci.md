# Reliability testing and CI

## Concurrency, performance, and failure tests

Use load tests to check behavior at expected traffic and stress tests to explore limits; define workload, duration, environment, and success thresholds before running. Benchmark a focused operation only when performance is relevant, and compare equivalent inputs/builds on a representative machine. A microbenchmark is not proof of whole-system capacity.

Test concurrency where correctness depends on simultaneous work: duplicate submissions, ordering, shared state, cache stampedes, database races, queue retries, and idempotency. Failure injection can check timeouts, dropped connections, provider errors, process restarts, and resource pressure, but run it only in an isolated authorized environment. Establish recovery criteria and verify the observed behavior. Never run destructive or high-load tests against production without explicit authorization and a reviewed scope.

Keep timeouts bounded. Test timeout and retry behavior without adding arbitrary delay. Retries need bounded attempts, backoff where appropriate, and safe/idempotent work. Avoid retries in test harnesses that hide flaky behavior.

## Flaky tests

A flaky test passes and fails without a relevant product change. Investigate timing assumptions, shared state, network reliance, ordering, cleanup, random data, resource limits, parallel collisions, and environment differences. Capture a reproduction and evidence, then fix the cause. Quarantine only through an explicit project policy with an owner and removal plan; do not silently skip it or lower assertions.

## Coverage and release confidence

Coverage reports locate unexecuted code and help identify gaps; they do not measure assertion quality, test isolation, specification completeness, or correctness. Focus review on important paths, branches, failures, and invariants. Do not add brittle tests merely to reach a percentage target.

Separate CI suites by feedback time and risk when useful: fast formatting/lint/typecheck/unit checks on each change; focused service integration checks where dependencies are available; broader browser, cross-service, performance, or release checks at an appropriate gate. This is a guide, not a mandatory pipeline. Parallelize independent tests only when fixtures, ports, databases, and resources are safely isolated. Publish reports/artifacts without exposing credentials or user data.

For GitHub Actions, inspect existing triggers and permissions, use lockfile-based installs and appropriate dependency caches, protect secrets from untrusted pull requests, and report which workflow jobs actually ran. Coordinate CI environments and deployments with `$devops-infrastructure`; a green CI workflow does not itself establish production readiness.

## Honest reporting

Use precise status terms:

- **Written:** test code exists; it may not have run.
- **Executed:** the named command/job ran in the stated environment.
- **Passed/failed:** the actual result of that execution.
- **Not run:** unavailable, out of scope, too risky, or not yet needed; say which and why when material.

Report skips, retries, flaky reruns, and environment limitations. Do not combine “tests added” with “tests passed” unless both are true.

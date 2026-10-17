# Backend Quality and Operations

Apply the checks that match the feature's risks and deployment context. This is a guide, not a universal release checklist.

## API contracts and versioning

- Specify request and response schemas, auth requirements, status/error semantics, pagination, and compatibility expectations. Generate OpenAPI or equivalent from the source of truth where practical; avoid a stale second contract.
- Prefer backward-compatible additive changes. Version when incompatible semantics or representations cannot be evolved safely; do not create a versioning framework before there is a compatibility need.
- Document authentication, limits, errors, and operationally meaningful behavior. Treat generated clients and external consumers as contract users.

## Testing

Use the narrowest test that proves a contract, then add broader coverage where boundaries or risks justify it:

- **Unit:** deterministic domain rules and pure transformations without infrastructure.
- **Integration:** route/service/database/cache/queue behavior with real or faithful dependencies, including constraints and transaction behavior that mocks cannot establish.
- **Contract:** compatibility between an API and its consumers or between a producer and worker payload schema.
- **End-to-end:** critical user flows across deployed-like boundaries. Keep these focused because they are slower and more environment-sensitive.

Test authorization failures, invalid inputs, expected error mapping, and important concurrency/retry/idempotency cases in proportion to risk. Avoid tests that merely mirror private implementation details or duplicate every layer. Use isolated data, deterministic clocks/IDs when useful, and cleanup that does not hide resource leaks.

## Logging and observability

- Emit structured logs with request/job/correlation identifiers and useful operation context. Redact secrets and sensitive values; set retention and access according to the data involved.
- Track request latency and error rates, database pool/slow query signals, queue depth/age/failure/retry counts, and cache health where those dependencies exist.
- Use traces across process boundaries when they help find latency or failures. Propagate trace context through queue messages deliberately.
- Add health/readiness checks that distinguish a process being alive from being ready to serve. Avoid exposing detailed dependency errors publicly.
- Make alerts actionable and tied to user impact or resource exhaustion; observability volume is not a substitute for diagnosis.

## Configuration and lifecycle

- Parse and validate configuration at startup, fail clearly for missing required values, and distinguish local defaults from production requirements.
- Inject secrets through an approved secret store/environment mechanism. Rotate credentials; restrict access; never commit `.env` secrets or print them during diagnostics.
- Set timeouts and resource limits for inbound requests, outbound calls, database pools, and job processing. Handle cancellation where the runtime and dependency support it.
- Close HTTP servers, database pools, queue consumers, and telemetry exporters during graceful shutdown. Use deployment termination windows that allow cleanup, while preserving a hard stop.

## Performance, scale, and deployment

- Establish workload expectations and measure bottlenecks before optimizing. Bound concurrency, request bodies, pagination, pools, and queue intake to protect dependencies.
- Look for N+1 queries, unbounded result sets, repeated serialization, inefficient indexes, blocking CPU work on the event loop, memory retention, and excessive external calls.
- Scale stateless API replicas horizontally only after accounting for shared state, connection limits, rate limits, and background work ownership. Add a worker process when workload isolation or independent scaling warrants it.
- Use health checks, rolling/blue-green strategy as appropriate, migrations compatible with deploy order, and rollback plans. A schema rollback may not be safe after writes; plan forward recovery and backups.
- Keep deployment configuration and runtime behavior reproducible. Document required services, migrations, worker startup, and shutdown; do not assume local development proves production readiness.

## Architecture review prompts

1. Can a reader trace a request/job from entrypoint through policy to persistence/effect without jumping across unrelated modules?
2. Is each abstraction earning its cost through reused policy, isolation, testability, or independent change?
3. Do module exports and dependencies prevent accidental coupling and circular imports?
4. Are data ownership and consistency invariants enforced under concurrency, retries, and partial failure?
5. Are security checks applied at every relevant entrypoint, including worker/admin operations?
6. Are errors, logs, metrics, and health checks enough to operate the feature without leaking sensitive information?
7. Do migration, API contract, and deployment steps work with mixed old/new versions during rollout?
8. Are tests located at boundaries where failures can actually occur, without excessive duplication?
9. Is any proposed service, cache, queue, abstraction, or technology justified by a concrete requirement?

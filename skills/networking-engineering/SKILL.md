---
name: networking-engineering
description: Design, implement, diagnose, secure, and verify software communication between applications, APIs, containers, and services.
---

# Networking Engineering

Use this skill when software communication fails or when designing connections among clients, APIs, real-time services, containers, databases, caches, proxies, and cloud services. It focuses on application and service networking, not general Cisco, carrier, or telecommunications administration. Principles are language-independent; version 1 has detailed initial guidance for Node.js/TypeScript, HTTP clients, WebSockets, Docker networking, PostgreSQL, MongoDB, and Redis.

## Workflow

1. Inspect the current communication path, topology, code/configuration, runtime versions, DNS names, ports, proxy/TLS boundaries, logs, and recent changes. Preserve established deployment behavior.
2. State the source, destination, protocol, environment, expected result, observed result, and exact point of failure. Separate a confirmed fact from a hypothesis.
3. Trace the path in order: name resolution -> route/interface -> transport connection -> TLS handshake -> protocol/request -> application/dependency response. Use the narrowest safe diagnostic that can distinguish layers.
4. Select the simplest fix that meets the requirements. Account for timeouts, cancellation, retries, idempotency, partial availability, pooling, backpressure, and graceful degradation.
5. Protect the boundary: retain certificate verification, restrict exposure and access, validate external endpoints, trust forwarded headers only from known proxies, and avoid leaking credentials or sensitive traffic in diagnostics.
6. Verify with a reproducible local or authorized integration test. Do not run intrusive scans, probe unknown hosts, expose services publicly, or alter live infrastructure without authorization.
7. Explain the failure path and fix in plain English. Report which checks ran and which conclusions remain assumptions.

## Engineering rules

- Diagnose the current network architecture before proposing a new one.
- Identify the exact caller-to-target path and the layer where behavior changes.
- Prefer the smallest networking solution that meets latency, availability, scale, and security needs.
- Use secure transport and certificate verification. Never disable TLS checks or weaken firewalls/access controls just to make a connection succeed.
- Bound connection/request timeouts and cancellation. Retry only appropriate transient failures, with bounded backoff and jitter; make writes idempotent or otherwise safe to repeat.
- Account for DNS changes, connection pools, partial failures, partitions, slow consumers, and service readiness.
- Do not expose a listener or publish a container port publicly without authorization and a clear need.
- Treat CORS as a browser permission policy, not a network connectivity test or server-side access control.
- Use only authorized, proportionate diagnostics. Read-only checks against a specified service differ from port scans, fuzzing, or testing unknown systems.
- Preserve deployment behavior unless a change is approved. Use connectivity/integration tests appropriate to the actual path.
- Distinguish observed output from interpretation and recommendations. Explain important choices in clear everyday English.

## References

Read only the material relevant to the task:

- [Networking fundamentals and HTTP](references/fundamentals-and-http.md) for IP/DNS/TCP/UDP, connection lifecycle, HTTP versions, TLS, clients, retries, proxies, and gateways.
- [Real-time communication](references/realtime.md) for WebSockets, Socket.IO, SSE, polling, reconnection, scaling, delivery, and backpressure.
- [Docker and service connectivity](references/docker-and-services.md) for container networking, Compose, PostgreSQL/MongoDB/Redis connections, and safe diagnostics.
- [Distributed systems, security, and troubleshooting](references/distributed-security-and-troubleshooting.md) for partial failures, observability, TLS/network security, diagnostic commands, and platform differences.
- [Scenario review](VALIDATION.md) for the six design exercises and which checks were not executed.

## Work with related skills

- Use `$engineering-workflow` for substantial implementation phases, repository checks, documentation, and Git checkpoints.
- Use `$backend-architecture` for application module boundaries, APIs, workers, and service ownership; this skill covers the communication path between those components.
- Use `$devops-infrastructure` for container builds, deployment, cloud environment, and service operations; this skill focuses on protocols, endpoints, and connectivity diagnosis.
- Use `$security-engineering` for detailed security requirements, threat modeling, SSRF controls, TLS policy, and access boundaries.
- Use `$database-engineering` for database models, queries, migrations, transaction behavior, and engine-specific operations; this skill addresses connection configuration and network reachability.
- Use `$caching-engineering` for cache strategy, freshness, invalidation, and Redis caching correctness.
- Use `$testing-engineering` for test selection and execution; this skill identifies the networking boundary that needs coverage.
- Use `$debugging-workflow` for root-cause investigation of an observed communication defect; use this skill for network-layer concepts and diagnostics.
- Use `$plain-english-explainer` for detailed teaching; this skill already asks for simple explanations of network failures and trade-offs.

## Invocation examples

- Use `$networking-engineering` with `$debugging-workflow` to find why this Node.js API cannot reach PostgreSQL in Docker Compose. Do not remove its volume or expose the port publicly.
- Diagnose Socket.IO disconnects behind this reverse proxy; check upgrade forwarding, idle timeouts, heartbeats, and whether sticky sessions are actually required by the chosen adapter.
- Trace intermittent DNS failures from this backend to a managed service using authorized read-only diagnostics.
- Review this HTTP client's timeout/retry behavior for duplicate-write risk and TLS certificate verification.

## Scope and limitations

This skill covers software communication paths, not general network-device administration. Exact behavior depends on operating system, runtime, protocol, provider, proxy, and network policy. Diagnostic commands can be inconclusive or filtered by firewalls. A design review or local connectivity check does not prove availability or security across production paths; verify in an authorized target-like environment.

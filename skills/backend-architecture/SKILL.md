---
name: backend-architecture
description: Design, build, extend, and review maintainable backend applications. Version 1 focuses on Node.js and TypeScript with Express, Fastify, PostgreSQL, MongoDB, Redis, and background workers. Use when backend structure, boundaries, persistence, APIs, or operational design materially affect the task.
---

# Backend Architecture

Use this skill when a backend task needs architectural decisions: starting an application, adding a feature that crosses meaningful boundaries, reviewing structure, or planning a justified refactor. It also applies to backend architecture in other languages; the principles are language-independent, while the implementation guidance in the references is currently Node.js and TypeScript focused.

Keep the design proportional to the application. Prefer the simplest structure that makes responsibilities, dependencies, and change boundaries clear. Do not impose a fixed directory tree, layer, framework, database, or deployment style. For existing projects, follow established conventions unless evidence shows they obstruct correctness, security, or maintainability; explain the reason and scope before broad changes.

## Workflow

1. Establish context: inspect the repository, instructions, existing conventions, data flows, runtime/deployment constraints, and the size and risk of the change. For existing projects, map the relevant path before proposing new structure.
2. Identify the important quality attributes and constraints: product behavior, data ownership, security, consistency, expected load, team familiarity, and operational capacity. State material assumptions.
3. Choose the least complex architecture that meets them. Start with a modular monolith for most small and medium applications. Add processes, queues, services, or stronger abstractions only for concrete isolation, scaling, reliability, ownership, or deployment needs.
4. Define module responsibilities and allowed dependencies before expanding implementation. Put code near the feature or capability it serves; centralize only genuinely shared policy or infrastructure.
5. Design the API and data changes together, including validation, authorization, failure semantics, transaction boundaries, indexes, migrations, and compatibility.
6. Plan tests and operational behavior at the boundaries that matter. Use the `engineering-workflow` skill for phased implementation, repository checks, documentation, and checkpoints. Use `debugging-workflow` when the primary task is diagnosing an observed failure.
7. Review the result against the checklist below. Record important tradeoffs and decisions in the project's existing documentation when useful.

For folder organization, layer responsibilities, feature-local versus shared code, and Express/Fastify layout differences, read [Project organization patterns](references/project-organization.md). Preserve existing structure unless a justified, explicitly scoped restructuring is part of the task.

## Architecture principles

- Organize by feature or business capability when that makes ownership and change easier to follow. A feature may contain its HTTP interface, application logic, domain rules, data access, and tests. Avoid a global `controllers/`, `services/`, `repositories/` tree that scatters every feature by technical type when that harms cohesion.
- Do not create a layer merely because a template includes it. A route may call a small use-case function directly; a repository is useful when it expresses meaningful persistence behavior, isolates a complex query model, or enables a real boundary. Keep simple code simple.
- Separate transport concerns (HTTP parsing, status codes, response shape) from business rules and persistence details when those concerns change independently. Keep domain rules independent of framework request/response objects.
- Keep dependencies pointing toward stable policy and contracts. Infrastructure and framework adapters implement or call inward-facing application capabilities; domain policy should not depend on Express/Fastify, a database driver, Redis, or environment globals. Avoid circular imports and hidden cross-feature access.
- Make shared modules small and intentional. A module should have a coherent reason to change, explicit public exports, and a clear owner. Prefer readable names and cohesive files over arbitrary line limits or one-class-per-file rules.
- Use consistent, searchable naming: follow repository language and casing conventions; name modules after their feature/capability and files after their role where helpful. Avoid ambiguous buckets such as `common`, `helpers`, or `misc` that accumulate unrelated behavior.
- Treat input, identity, authorization, persistence, and external effects as explicit boundaries. Fail safely and observably; do not let untrusted input or client claims establish authority.
- Model data consistency deliberately. Define transaction scope around an invariant, not around an entire request by habit. Use idempotency, unique constraints, outbox or reconciliation patterns where retries and cross-system effects require them.
- Prefer explicit configuration and dependency composition at application startup. Validate required configuration once, keep secrets outside source control and logs, and make resource lifecycle and shutdown behavior clear.

## Node.js and TypeScript guidance

Read [Node.js and TypeScript patterns](references/node-typescript.md) for practical Express/Fastify boundaries, validation and errors, authentication, persistence, Redis, workers, and file organization.

Read [Backend quality and operations](references/quality-and-operations.md) for API contracts, testing, observability, performance, deployment, and review checks.

Read [System-design decisions](references/system-design-decisions.md) when comparing architecture options, setting service boundaries, planning for growth or failure, or recording a significant trade-off. Use it as decision guidance, not a required architecture template.

## Review checklist

Before recommending or completing a design, check that:

- The structure fits actual complexity and preserves useful project conventions.
- Each directory and file has a clear purpose; feature code is cohesive and public module surfaces are deliberate.
- Dependencies flow in a comprehensible direction, with framework and infrastructure details at appropriate boundaries.
- Validation occurs at external inputs; authentication establishes identity and authorization checks the requested action on the target resource.
- Persistence constraints, transaction boundaries, migrations, indexes, and consistency behavior support the stated invariants.
- Cache and queue behavior accounts for staleness, failure, duplicate delivery, retries, and idempotency where relevant.
- Errors have stable client semantics without leaking internals; logs and metrics provide useful context without secrets or sensitive payloads.
- Tests cover important behavior at appropriate levels, and API contracts/documentation match implementation.
- Performance and deployment choices address measured or credible requirements without premature distribution or tuning.
- Tradeoffs, limitations, and any meaningful migration path are clear.

## Invocation examples

- “Use `$backend-architecture` to design a small Express + TypeScript API for projects and tasks. Keep it a modular monolith and explain the module boundaries.”
- “Use `$backend-architecture` to review this Fastify/PostgreSQL feature for transaction, validation, and dependency-boundary issues.”
- “Use `$backend-architecture` with `$engineering-workflow` to add password reset to this API while preserving its current layout.”
- “Use `$debugging-workflow` to find why this worker processes some jobs twice; use `$backend-architecture` only if the root cause requires an architecture change.”

## Scope and limitations

Version 1 provides practical defaults for Node.js and TypeScript, especially Express, Fastify, PostgreSQL, MongoDB, Redis, and background workers. It does not prescribe a universal framework, ORM, folder layout, cloud provider, microservice pattern, or compliance regime. For other languages, apply the architecture principles and consult the ecosystem's authoritative conventions and security guidance rather than translating TypeScript-specific examples literally.

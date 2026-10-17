---
name: devops-infrastructure
description: Design, implement, review, and troubleshoot container, CI/CD, deployment, observability, and reliability workflows.
---

# DevOps and Infrastructure

Use this skill when a change affects how an application is built, configured, run, deployed, monitored, or recovered. It supports new and existing projects. Principles apply across languages and cloud platforms; version 1 gives detailed initial guidance for Node.js/TypeScript, Docker, Docker Compose, GitHub Actions, PostgreSQL, MongoDB, Redis, and common cloud deployment environments.

## Workflow

1. Inspect repository instructions, current Docker/build files, CI workflows, deployment targets, runtime versions, environment configuration, service topology, health signals, tests, and recent operational evidence. Preserve established behavior unless change is justified and authorized.
2. Establish environment boundaries: what is local development, CI, staging, and production; who owns each service; what data is persistent; what availability, security, recovery, and cost constraints matter.
3. Choose the simplest deployment and service model that meets these requirements. Prefer managed services when they reduce operational burden and fit cost/control needs; self-host only with a clear ownership and maintenance plan.
4. Plan image/build, configuration/secrets, networking, startup/readiness, storage, resource limits, rollout, migrations, observability, shutdown, and recovery as connected concerns.
5. Implement only within the authorized environment. Test configuration locally or in isolated CI/staging as appropriate. Never deploy, modify live infrastructure, rotate production secrets, or perform destructive container/storage operations without explicit approval.
6. Verify the delivered configuration with the available checks and target-like environment. Clearly separate a static review or design exercise from a Docker build, CI run, deployment, restore, or production observation.
7. Explain operational trade-offs and costs in plain English. State what was tested, what is only recommended, and what remains unknown.

## Operating rules

- Prefer the smallest infrastructure that meets actual requirements. Do not add an orchestrator, service, queue, managed product, or deployment layer without a concrete need.
- Distinguish local development convenience from production requirements. Compose examples are not production deployment plans by default.
- Keep secrets out of source, Docker build contexts/images, workflow logs, and command output. Use protected environment or secret-manager mechanisms with least privilege.
- Use secure container and CI defaults: minimal needed image contents, non-root runtime where feasible, explicit workflow permissions, maintained dependencies/actions, and controlled deployment access.
- Preserve deployment behavior unless a change is approved and justified. Avoid destructive Docker commands, image/data cleanup, and volume removal without explicit authorization.
- Never deploy, publish, rotate production credentials, or modify live infrastructure without approval. Validate the target, impact, rollout, and recovery plan before any approved operation.
- Coordinate schema/data changes with the application rollout. A database rollback may not undo data writes; prefer compatible migrations and forward repair where appropriate.
- Include meaningful health/readiness behavior and deployment verification. A running process is not necessarily ready to serve traffic.
- Explain operational cost, ownership, failure modes, and trade-offs. Do not claim a configuration works unless it was tested in the relevant environment.

## References

Read only what the task needs:

- [Containers and Compose](references/containers.md) for Dockerfiles, image builds, local Compose services, networking, persistence, and startup troubleshooting.
- [CI/CD with GitHub Actions](references/ci-cd.md) for workflow quality gates, artifacts, permissions, secrets, and deployment gates.
- [Deployment and migrations](references/deployment.md) for environment promotion, cloud choices, rollout, migration sequencing, verification, and recovery.
- [Observability and reliability](references/operations.md) for logs, metrics, traces, health, shutdown, capacity, runbooks, incidents, and restore testing.
- [Scenario review](VALIDATION.md) for the five design exercises and their limits.

## Work with related skills

- Use `$engineering-workflow` for phased implementation, repository checks, documentation, and safe local Git checkpoints.
- Use `$backend-architecture` for application module boundaries, request flows, worker design, and application-level operational choices.
- Use `$database-engineering` for schema, query, consistency, migration, backup, and engine-specific database operations. This skill handles deployment coordination and service operations around those databases.
- Use `$security-engineering` for threat modeling and detailed security controls. This skill applies secure operational defaults to containers, workflows, and deployments.
- Use `$authentication-engineering` for identity providers, credentials, sessions, and access decisions.
- Use `$caching-engineering` for cache strategy, freshness, invalidation, and cache correctness; this skill handles Redis service operations and availability.
- Use `$language-engineering` for language-level implementation and runtime compatibility.
- Use `$debugging-workflow` to find the root cause of observed build, CI, container, or deployment failures before changing configuration.
- Use `$plain-english-explainer` for detailed teaching; this skill asks for clear operational explanations by default.

## Invocation examples

- Use `$devops-infrastructure` with `$backend-architecture` to containerize this Node.js/TypeScript API and its local PostgreSQL/Redis dependencies without treating Compose as production deployment.
- Review this GitHub Actions pipeline for missing quality gates, overbroad permissions, secret exposure, and uncontrolled deployment steps.
- Plan staging-to-production rollout for this service, including backward-compatible migrations, health checks, and rollback or forward recovery.
- Use `$debugging-workflow` to investigate why this Compose service is unhealthy, then use `$devops-infrastructure` to make an evidence-based configuration fix.

## Scope and limitations

Version 1 includes operational guidance for Docker/Compose and GitHub Actions, with portable principles for common cloud environments and PostgreSQL, MongoDB, and Redis services. It does not prescribe a cloud provider, production topology, or infrastructure-as-code tool. Exact configuration depends on the project's versions, hosting platform, workload, data policy, and recovery objectives. Static review does not prove a deployment, backup, or recovery procedure works.

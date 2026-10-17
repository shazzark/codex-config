# DevOps and infrastructure scenario review

These are design exercises evaluating whether the skill gives proportionate, operationally safe guidance. They are not Docker builds, GitHub Actions runs, deployments, restore tests, or live-service observations.

| Scenario | Appropriate guidance and review result | Check type |
|---|---|---|
| Containerize a Node.js/TypeScript API with PostgreSQL and Redis | Inspect Node version/lockfile/build output; use a suitable multi-stage image and `.dockerignore`; runtime as non-root where feasible; separate secrets; use Compose for local services, health/readiness, persistent database volume, service DNS, resource assumptions, and application retry behavior. Let database/cache references own persistence and cache semantics. | Design exercise; no Dockerfile built or service started. |
| GitHub Actions pipeline for Next.js | Inspect scripts and supported versions; run lint/typecheck/tests/build; cache by lockfile; grant least token permission; protect secrets from untrusted PR code; promote tested artifact and gate production with environment protections. | Design exercise; no workflow run or GitHub configuration inspected. |
| Production deployment with migrations and rollback | Identify environment and artifact, stage compatible schema changes before dependent code, verify health/critical paths, define stop/rollback or forward-repair criteria, and require approval before live deployment. Confirm backup/restore evidence separately. | Design exercise; no migration, deployment, or rollback executed. |
| Investigate unhealthy Compose service | Inspect health status, sanitized logs, exit code, app listen address/port, dependency readiness, service DNS, mounted path/permissions, configuration presence, resource use, and recent changes. Avoid volume deletion and broad cleanup as diagnostics. | Design exercise; no Compose project or logs supplied. |
| Monitor and recover multi-service backend | Choose actionable service and dependency signals, request correlation, readiness/liveness separation, bounded retries/timeouts, graceful shutdown, capacity indicators, runbooks, backup/restore evidence, and incident review. | Design exercise; no deployed backend, alert, or restore test. |

## Findings and limitations

The scenarios keep application module design, database schema safety, and security threat controls with their specialist skills while giving infrastructure decisions a clear owner. The guidance distinguishes local Compose from production, and running containers from ready services. Design reviews alone do not prove image behavior, workflow permissions, deployment safety, service availability, backup validity, or recoverability. Verify with the actual repository, target platform, Docker/CI environment, and authorized isolated or production process.

# Codex Configuration Skills

This repository contains reusable Codex skills and their supporting references. Skills here are maintained as source and synchronized to the user's global Codex skills directory at `$HOME/.codex/skills`.

## Skills

### `review-agent`

Reviews a completed change or delivery phase against its approved requirements and acceptance criteria. It inspects relevant code and Git diffs, investigates potential defects using evidence, runs practical non-destructive checks, and reports prioritized findings, verification results, unresolved risks, and an acceptance verdict. Findings distinguish confirmed defects, plausible risks, and optional improvements. The workflow is read-only by default and does not modify code, commit, push, deploy, delete files, install dependencies, or expose secrets.

Example: Use `$review-agent` to compare this completed phase with its approved acceptance criteria, inspect the relevant diff and code context, run safe checks where practical, and return a prioritized report.

The skill includes a review checklist, standardized report template, integration guidance for the existing engineering skills, and validation instructions. A documented read-only review exercise is in [VALIDATION.md](skills/review-agent/VALIDATION.md). Run the official skill-creator validator in Python UTF-8 mode as described in [the validation instructions](skills/review-agent/references/validation.md).

### `engineering-workflow`

Coordinates substantial repository work through inspection, proportional planning, incremental implementation, verification, review, documentation, and safe local checkpoints. Use it for features, refactors, or multi-phase changes; it is not needed for trivial edits or simple questions.

Example: Use `$engineering-workflow` to implement this feature in phases, run the relevant checks, review the diff, and create a focused local checkpoint if verification succeeds.

### `debugging-workflow`

Investigates software defects by reproducing symptoms, tracing evidence, testing hypotheses, finding the root cause, applying a focused fix, and verifying it with regression checks. Use it when diagnosing why behavior is broken; combine it with a domain skill when the confirmed cause needs specialized design guidance.

Example: Use `$debugging-workflow` to find why this API intermittently returns 500, then use `$database-engineering` if evidence points to a query or transaction problem.

### `project-planning`

Turns a software idea or existing-project request into clarified requirements, a proportionate MVP, selected planning documents, and an actionable phased plan. Quick mode suits small utilities; Standard mode suits typical web/mobile/backend products; Advanced mode covers justified complexity such as multi-service systems and AI agents. It does not implement application code.

Example: Use `$project-planning` in Standard mode to plan this mobile app, ask focused questions about material unknowns, propose only relevant documents (including a TRD when technical constraints warrant one), and stop for approval before implementation.

The skill includes reusable templates for project briefs, MVP scope, PRDs, TRDs, app flows, architecture, data, design systems, integrations, risks, testing/acceptance, ADRs, implementation phases, and tasks. Keep assumptions distinct from approved requirements. Planning exercises are not stakeholder validation, feasibility proof, or permission to create repositories, install dependencies, configure cloud services, or deploy. The complete skill installs under `$HOME/.codex/skills/project-planning`.

### `authentication-engineering`

Designs and reviews authentication and authorization systems, including account lifecycle, passwords, sessions, JWTs, OAuth/OIDC, MFA, passkeys, and resource/tenant access control. Version 1 gives language-independent principles with initial Node.js, TypeScript, Express, Fastify, Next.js, PostgreSQL, MongoDB, and Redis guidance. It favors maintained identity providers and libraries and does not require custom authentication.

Examples: Use `$authentication-engineering` with `$backend-architecture` to review this Next.js OIDC login and verify access checks near the data layer. Use `$authentication-engineering` and `$database-engineering` to design refresh-token rotation and revocation storage. Pair it with `$security-engineering` for broader application threats, `$engineering-workflow` for implementation, `$debugging-workflow` for defects, and `$plain-english-explainer` for teaching. Skill sources and references install together under `$HOME/.codex/skills/authentication-engineering`.

Limitations: this skill guides design and review; it is not a compliance certification, penetration test, or assurance that a system is secure. Provider and framework behavior changes, so confirm current documentation and test the target configuration.

### `security-engineering`

Threat-models and secures applications, APIs, data paths, dependencies, containers, CI/CD, and deployments. Version 1 provides language-independent principles with detailed guidance for Node.js, TypeScript, React, Next.js, Express, Fastify, PostgreSQL, MongoDB, and Redis. It covers common OWASP web/API risks, input and output handling, authorization boundaries, secrets, integrations, testing, and vulnerability response.

Example: Use `$security-engineering` with `$engineering-workflow` to review this API's authorization and add security regression checks. Pair it with `$authentication-engineering` for identity flows, `$database-engineering` for database design, `$backend-architecture` for module boundaries, and `$debugging-workflow` for defect investigation. Install the complete skill directory at `$HOME/.codex/skills/security-engineering`.

Limitations: scenario reviews and automated scans do not prove a system is secure. Security testing must be authorized and scoped; verify current framework, provider, and jurisdiction-specific requirements for each application.

### `caching-engineering`

Designs and reviews caches across Node.js, TypeScript, Express, Fastify, Next.js, PostgreSQL, MongoDB, Redis, HTTP, CDNs, and TanStack Query. It covers freshness, invalidation, stampedes, failure handling, cache isolation, observability, and performance verification. Caching is recommended only when requirements and measurements justify its complexity.

Example: Use `$caching-engineering` with `$database-engineering` to plan invalidation after a write, or with `$backend-architecture` to decide where a cache belongs. Pair it with `$security-engineering` for private-data isolation, `$debugging-workflow` for stale-data/performance defects, and `$engineering-workflow` for a verified implementation. The complete skill installs under `$HOME/.codex/skills/caching-engineering`.

Limitations: design exercises and cache metrics alone do not demonstrate correct invalidation or faster behavior. Verify against the actual workload, framework versions, and deployment topology.

### `database-engineering`

Designs, implements, reviews, optimizes, secures, and maintains database systems. Version 1 covers PostgreSQL, MySQL, MongoDB, and Redis, with reusable principles for other data stores. It focuses on database models, queries, transactions, migrations, security, and operational reliability; it does not force one engine or schema design.

Redis responsibility: use `$caching-engineering` for cache strategy, freshness, invalidation, and request-path behavior. Use `$database-engineering` for Redis data structures, durable state, persistence, eviction, security, and operations. Combine them when Redis-specific limits or failure behavior affect cache correctness.

Example: “Use `$database-engineering` with `$engineering-workflow` to plan a backward-compatible PostgreSQL migration and verify it against the test database.” The complete skill and technology references are installed together under `$HOME/.codex/skills/database-engineering`.

### `plain-english-explainer`

Explains technical concepts, code, errors, architecture, and implementation results in clear everyday English while preserving exact technical details and accuracy. It is also the default communication preference in the global `$HOME/.codex/AGENTS.md` instructions.

Invoke it to learn a concept or request a focused explanation. Example: “Use `$plain-english-explainer` to explain this TypeScript error and the safest fix.” Routine implementation reports stay concise and distinguish checks that ran from work that was not verified.

### `code-documentation`

Adds, updates, reviews, or explains code documentation grounded in the implementation. It covers useful source comments, language-appropriate docstrings and API comments, focused repository docs, and plain-English explanations of selected code. It emphasizes accurate public contracts, non-obvious rationale, project conventions, and keeping documentation changes separate from runtime behavior.

Example: Use `$code-documentation` to document the public contract and retry constraints of this client without changing its behavior, or to explain how the selected function moves data through the module.

The skill includes language convention references, a documentation review checklist, and validation instructions. Use `$plain-english-explainer` for broader technical teaching; use `$review-agent` for a broader review against approved requirements.

### `backend-architecture`

Designs, builds, extends, and reviews maintainable backend applications. Version 1 specializes in Node.js and TypeScript, especially Express, Fastify, PostgreSQL, MongoDB, Redis, and background workers. It also provides requirement-driven system-design guidance for service boundaries, scaling, distributed communication, reliability, operational fit, and architecture decisions. Its principles remain useful across languages, but detailed implementation examples and ecosystem guidance are currently Node.js focused.

Invoke it for new backend designs, meaningful boundary decisions, architecture reviews, or changes that cross backend responsibilities. Example: “Use `$backend-architecture` to add a Fastify endpoint while preserving this project's module boundaries.”

The skill covers proportionate architecture, feature cohesion, dependency direction, API and persistence boundaries, validation, security, transactions, indexes, caches, queues, workers, testing, observability, configuration, performance, deployment, and system-level trade-offs. It does not require a fixed folder layout, a repository layer, microservices, or any particular ORM or cloud provider.

Pair it with `$engineering-workflow` for substantial phased implementation, verification, documentation, and Git checkpoints. Pair it with `$debugging-workflow` when investigating an observed failure; use architecture guidance only if evidence points to a structural cause. The backend skill guides design, while those workflow skills retain execution and diagnostic responsibilities.

Scenario exercises are recorded in [skills/backend-architecture/VALIDATION.md](skills/backend-architecture/VALIDATION.md). Detailed guidance lives in [Node.js and TypeScript patterns](skills/backend-architecture/references/node-typescript.md) and [quality and operations](skills/backend-architecture/references/quality-and-operations.md).

### `design-system-engineering`

Plans, implements, maintains, and reviews consistent accessible interfaces and design systems. Its principles are framework-independent, with initial guidance for React, Next.js, TypeScript, Tailwind CSS, React Native, Expo, and Figma-to-code workflows. It covers semantic tokens, themes, responsive foundations, reusable components, accessibility, platform differences, design-system maintenance, and verification.

Example: Use `$design-system-engineering` to improve consistency in this React/Tailwind app while preserving its visual identity, checking responsive layouts, and adding accessible component states. For Figma work, pair it with the relevant installed Figma workflow skill and tools.

Limitations: design exercises do not prove accessibility conformance, device/browser behavior, or visual fidelity. Verify the actual implementation with the appropriate automated and manual checks; do not claim pixel-perfect matching without a visual comparison. The complete skill installs under `$HOME/.codex/skills/design-system-engineering`.

### `language-engineering`

Helps Codex write, refactor, review, and test readable, idiomatic, type-safe code. Its core principles apply across languages, with detailed version 1 guidance for TypeScript and JavaScript, including strict typing, runtime input validation, async behavior, modules, errors, compatibility, and quality checks.

Examples: Use `$language-engineering` to refactor this JavaScript module into strict TypeScript while preserving behavior. Review this TypeScript API for useful type boundaries and unnecessarily complex generics. Pair with `$debugging-workflow` to investigate an async failure and add a regression test.

Limitations: detailed guidance currently covers TypeScript and JavaScript only. Python, Go, PHP, Java, and other languages have not yet received language-specific references; consult their current ecosystem documentation and project conventions.

### `devops-infrastructure`

Plans, implements, reviews, and troubleshoots container builds, local Compose services, GitHub Actions, deployments, observability, reliability, and operational recovery. It provides language-independent principles with initial Node.js/TypeScript, Docker/Compose, PostgreSQL, MongoDB, Redis, GitHub Actions, and cloud deployment guidance.

Examples: Use `$devops-infrastructure` with `$backend-architecture` to containerize this API and its local services. Review this GitHub Actions workflow for least privilege and deployment gates. Plan a production rollout with `$database-engineering` for migration safety.

Limitations: it does not prescribe a cloud provider or production topology, and a Compose setup is not automatically production-ready. Design reviews do not prove that a build, deployment, backup, or restore works; use the target environment and report only checks actually performed. The complete skill installs under `$HOME/.codex/skills/devops-infrastructure`.

### `testing-engineering`

Helps Codex choose, write, run, debug, and maintain meaningful tests across frontend, backend, APIs, databases, distributed services, and mobile apps. Initial detailed guidance covers TypeScript/JavaScript, Node.js, React/Next.js, Express/Fastify, PostgreSQL, MongoDB, Redis, and React Native, including Vitest, Jest, React Testing Library, Supertest, Fastify injection, and Playwright.

Examples: Use `$testing-engineering` to test this Fastify endpoint against an isolated PostgreSQL database. Review these Playwright tests for realistic user flows and brittle selectors. Pair with `$debugging-workflow` to diagnose this flaky async test.

Limitations: test plans and passing suites cover only their exercised behaviors. They do not prove production correctness, complete security, capacity, accessibility, or recovery. The skill requires reporting exactly which checks ran; use isolated resources and do not target production data without explicit authorization.

### `networking-engineering`

Designs, implements, diagnoses, secures, and verifies communication between applications and services. It provides language-independent principles with initial guidance for Node.js/TypeScript, HTTP clients, WebSockets and Socket.IO, Docker networking, PostgreSQL, MongoDB, Redis, reverse proxies, and distributed services. It covers DNS, transport and TLS, retries, connection lifecycle, service discovery, real-time connections, safe diagnostics, and failure handling.

Examples: Use `$networking-engineering` with `$debugging-workflow` to trace why a Node.js API cannot reach PostgreSQL inside Docker Compose. Review this HTTP client's timeout and retry behavior for duplicate-write risk. Diagnose Socket.IO disconnects behind a reverse proxy without assuming sticky sessions are required.

Limitations: this skill focuses on software communication paths, not general network-device or telecommunications administration. Scenario reviews and local checks do not prove production reachability; do not probe unauthorized systems or weaken TLS, firewalls, or access controls to restore connectivity. Install the complete skill directory at `$HOME/.codex/skills/networking-engineering`.

## Installation

Each skill is a directory containing `SKILL.md` and optional references. Install a skill by copying its complete directory from `skills/` into the global Codex skills folder. This makes it available across repositories for your user account.

Before updating an existing installation, inspect or back it up. The examples below stop if the destination already exists, so they do not silently overwrite a global skill. Replace `project-planning` with any directory name under `skills/`.

### Windows PowerShell

```powershell
$ErrorActionPreference = "Stop"
$skill = "project-planning"
$source = Join-Path (Get-Location) "skills\$skill"
$destination = Join-Path $env:USERPROFILE ".codex\skills\$skill"
if (-not (Test-Path -LiteralPath $source -PathType Container)) { throw "Skill source not found: $source" }
if (Test-Path -LiteralPath $destination) { throw "Destination exists; back it up and inspect it before updating: $destination" }
New-Item -ItemType Directory -Path (Split-Path -Parent $destination) -Force | Out-Null
Copy-Item -LiteralPath $source -Destination $destination -Recurse
```

### macOS and Linux

```sh
skill=project-planning
source="./skills/$skill"
destination="$HOME/.codex/skills/$skill"
test -d "$source" || { printf 'Skill source not found: %s\n' "$source" >&2; exit 1; }
if [ -e "$destination" ]; then
  printf 'Destination exists; back it up and inspect it before updating: %s\n' "$destination" >&2
  exit 1
fi
mkdir -p "$(dirname "$destination")"
cp -R "$source" "$destination"
```

The standard global location is `$HOME/.codex/skills` (PowerShell can use `$env:USERPROFILE\.codex\skills`). Copying a skill folder includes its `SKILL.md` and references. Restart Codex or start a new session if the skill list does not refresh. Keep project-specific skills in the repository when they should not be available globally.

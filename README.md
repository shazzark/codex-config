# Codex Configuration Skills

This repository contains reusable Codex skills and their supporting references. Skills here are maintained as source and synchronized to the user's global Codex skills directory at `C:\Users\chido\.codex\skills`.

## Skills

### `authentication-engineering`

Designs and reviews authentication and authorization systems, including account lifecycle, passwords, sessions, JWTs, OAuth/OIDC, MFA, passkeys, and resource/tenant access control. Version 1 gives language-independent principles with initial Node.js, TypeScript, Express, Fastify, Next.js, PostgreSQL, MongoDB, and Redis guidance. It favors maintained identity providers and libraries and does not require custom authentication.

Examples: Use `$authentication-engineering` with `$backend-architecture` to review this Next.js OIDC login and verify access checks near the data layer. Use `$authentication-engineering` and `$database-engineering` to design refresh-token rotation and revocation storage. Pair it with `$security-engineering` for broader application threats, `$engineering-workflow` for implementation, `$debugging-workflow` for defects, and `$plain-english-explainer` for teaching. Skill sources and references install together under `C:\Users\chido\.codex\skills\authentication-engineering`.

Limitations: this skill guides design and review; it is not a compliance certification, penetration test, or assurance that a system is secure. Provider and framework behavior changes, so confirm current documentation and test the target configuration.

### `security-engineering`

Threat-models and secures applications, APIs, data paths, dependencies, containers, CI/CD, and deployments. Version 1 provides language-independent principles with detailed guidance for Node.js, TypeScript, React, Next.js, Express, Fastify, PostgreSQL, MongoDB, and Redis. It covers common OWASP web/API risks, input and output handling, authorization boundaries, secrets, integrations, testing, and vulnerability response.

Example: Use `$security-engineering` with `$engineering-workflow` to review this API's authorization and add security regression checks. Pair it with `$authentication-engineering` for identity flows, `$database-engineering` for database design, `$backend-architecture` for module boundaries, and `$debugging-workflow` for defect investigation. Install the complete skill directory at `C:\Users\chido\.codex\skills\security-engineering`.

Limitations: scenario reviews and automated scans do not prove a system is secure. Security testing must be authorized and scoped; verify current framework, provider, and jurisdiction-specific requirements for each application.

### `caching-engineering`

Designs and reviews caches across Node.js, TypeScript, Express, Fastify, Next.js, PostgreSQL, MongoDB, Redis, HTTP, CDNs, and TanStack Query. It covers freshness, invalidation, stampedes, failure handling, cache isolation, observability, and performance verification. Caching is recommended only when requirements and measurements justify its complexity.

Example: Use `$caching-engineering` with `$database-engineering` to plan invalidation after a write, or with `$backend-architecture` to decide where a cache belongs. Pair it with `$security-engineering` for private-data isolation, `$debugging-workflow` for stale-data/performance defects, and `$engineering-workflow` for a verified implementation. The complete skill installs under `C:\Users\chido\.codex\skills\caching-engineering`.

Limitations: design exercises and cache metrics alone do not demonstrate correct invalidation or faster behavior. Verify against the actual workload, framework versions, and deployment topology.

### `database-engineering`

Designs, implements, reviews, optimizes, secures, and maintains database systems. Version 1 covers PostgreSQL, MySQL, MongoDB, and Redis, with reusable principles for other data stores. It focuses on database models, queries, transactions, migrations, security, and operational reliability; it does not force one engine or schema design.

Example: “Use `$database-engineering` with `$engineering-workflow` to plan a backward-compatible PostgreSQL migration and verify it against the test database.” The complete skill and technology references are installed together under `C:\Users\chido\.codex\skills\database-engineering`.

### `plain-english-explainer`

Explains technical concepts, code, errors, architecture, and implementation results in clear everyday English while preserving exact technical details and accuracy. It is also the default communication preference in the global `C:\Users\chido\.codex\AGENTS.md` instructions.

Invoke it to learn a concept or request a focused explanation. Example: “Use `$plain-english-explainer` to explain this TypeScript error and the safest fix.” Routine implementation reports stay concise and distinguish checks that ran from work that was not verified.

### `backend-architecture`

Designs, builds, extends, and reviews maintainable backend applications. Version 1 specializes in Node.js and TypeScript, especially Express, Fastify, PostgreSQL, MongoDB, Redis, and background workers. Its architecture principles remain useful across languages, but detailed examples and ecosystem guidance are currently Node.js focused.

Invoke it for new backend designs, meaningful boundary decisions, architecture reviews, or changes that cross backend responsibilities. Example: “Use `$backend-architecture` to add a Fastify endpoint while preserving this project's module boundaries.”

The skill covers proportionate architecture, feature cohesion, dependency direction, API and persistence boundaries, validation, security, transactions, indexes, caches, queues, workers, testing, observability, configuration, performance, and deployment. It does not require a fixed folder layout, a repository layer, microservices, or any particular ORM or cloud provider.

Pair it with `$engineering-workflow` for substantial phased implementation, verification, documentation, and Git checkpoints. Pair it with `$debugging-workflow` when investigating an observed failure; use architecture guidance only if evidence points to a structural cause. The backend skill guides design, while those workflow skills retain execution and diagnostic responsibilities.

Scenario exercises are recorded in [skills/backend-architecture/VALIDATION.md](skills/backend-architecture/VALIDATION.md). Detailed guidance lives in [Node.js and TypeScript patterns](skills/backend-architecture/references/node-typescript.md) and [quality and operations](skills/backend-architecture/references/quality-and-operations.md).

### `design-system-engineering`

Plans, implements, maintains, and reviews consistent accessible interfaces and design systems. Its principles are framework-independent, with initial guidance for React, Next.js, TypeScript, Tailwind CSS, React Native, Expo, and Figma-to-code workflows. It covers semantic tokens, themes, responsive foundations, reusable components, accessibility, platform differences, design-system maintenance, and verification.

Example: Use `$design-system-engineering` to improve consistency in this React/Tailwind app while preserving its visual identity, checking responsive layouts, and adding accessible component states. For Figma work, pair it with the relevant installed Figma workflow skill and tools.

Limitations: design exercises do not prove accessibility conformance, device/browser behavior, or visual fidelity. Verify the actual implementation with the appropriate automated and manual checks; do not claim pixel-perfect matching without a visual comparison. The complete skill installs under `C:\Users\chido\.codex\skills\design-system-engineering`.

## Installation

Each skill is a directory containing `SKILL.md` and optional references. Synchronize the complete skill directory to the corresponding name under `C:\Users\chido\.codex\skills`; do not replace unrelated global skills. Verify the installed tree against the repository source after synchronization.

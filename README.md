# Codex Configuration Skills

This repository contains reusable Codex skills and their supporting references. Skills here are maintained as source and synchronized to the user's global Codex skills directory at `C:\Users\chido\.codex\skills`.

## Skills

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

## Installation

Each skill is a directory containing `SKILL.md` and optional references. Synchronize the complete skill directory to the corresponding name under `C:\Users\chido\.codex\skills`; do not replace unrelated global skills. Verify the installed tree against the repository source after synchronization.

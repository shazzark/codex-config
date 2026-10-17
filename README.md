# Codex Configuration Skills

This repository contains reusable Codex skills and their supporting references. Skills here are maintained as source and synchronized to the user's global Codex skills directory at `C:\Users\chido\.codex\skills`.

## Skills

### `backend-architecture`

Designs, builds, extends, and reviews maintainable backend applications. Version 1 specializes in Node.js and TypeScript, especially Express, Fastify, PostgreSQL, MongoDB, Redis, and background workers. Its architecture principles remain useful across languages, but detailed examples and ecosystem guidance are currently Node.js focused.

Invoke it for new backend designs, meaningful boundary decisions, architecture reviews, or changes that cross backend responsibilities. Example: “Use `$backend-architecture` to add a Fastify endpoint while preserving this project's module boundaries.”

The skill covers proportionate architecture, feature cohesion, dependency direction, API and persistence boundaries, validation, security, transactions, indexes, caches, queues, workers, testing, observability, configuration, performance, and deployment. It does not require a fixed folder layout, a repository layer, microservices, or any particular ORM or cloud provider.

Pair it with `$engineering-workflow` for substantial phased implementation, verification, documentation, and Git checkpoints. Pair it with `$debugging-workflow` when investigating an observed failure; use architecture guidance only if evidence points to a structural cause. The backend skill guides design, while those workflow skills retain execution and diagnostic responsibilities.

Scenario exercises are recorded in [skills/backend-architecture/VALIDATION.md](skills/backend-architecture/VALIDATION.md). Detailed guidance lives in [Node.js and TypeScript patterns](skills/backend-architecture/references/node-typescript.md) and [quality and operations](skills/backend-architecture/references/quality-and-operations.md).

## Installation

Each skill is a directory containing `SKILL.md` and optional references. Synchronize the complete skill directory to the corresponding name under `C:\Users\chido\.codex\skills`; do not replace unrelated global skills. Verify the installed tree against the repository source after synchronization.

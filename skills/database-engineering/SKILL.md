---
name: database-engineering
description: Design, implement, review, optimize, secure, and operate reliable database systems. Version 1 focuses on PostgreSQL, MySQL, MongoDB, and Redis, with principles that also apply to other data stores. Use when a task changes database choice, data models, queries, migrations, consistency, security, or database operations.
---

# Database Engineering

Use this skill when database design or behavior is central to the task. Select a database and design its data around the application's access patterns, consistency needs, integrity rules, operational capacity, and existing system. Do not force a particular engine, schema style, ORM, index, or abstraction without a concrete reason.

The detailed references cover [relational databases](references/relational-databases.md), [MongoDB](references/mongodb.md), [Redis](references/redis.md), and [cross-database operations and safety](references/operations-and-safety.md).

## Workflow

1. Inspect the existing database engines, schemas, migrations, data ownership, query paths, configuration, tests, and deployment conventions before proposing changes. Preserve existing data and useful conventions.
2. Clarify the workload and requirements: important reads/writes, relationships, query flexibility, consistency, volume and growth, latency, availability, retention, privacy, team skills, and operating constraints. Identify unknowns rather than inventing them.
3. Select the simplest suitable database and model. Explain the decision and practical trade-offs. Relational and document models can coexist when ownership and operational cost are clear; avoid adding a second store without a need.
4. Design constraints, queries, transactions, indexes, and failure behavior together. Understand actual access patterns before adding indexes or denormalizing data.
5. Plan schema and data changes for compatible deployments, bounded work, recovery, and safe rollback or forward repair. Never assume a schema rollback can restore changed or deleted data.
6. Protect credentials and records, use least privilege, and distinguish local/test actions from production actions. Never run production migrations or modify production data without explicit authorization. Do not perform destructive database actions without explicit approval.
7. Add appropriate database tests and verify migrations and queries against a suitable isolated test environment when available. Report what actually ran and what remains assumed or unverified.
8. Explain the database decisions and trade-offs in clear language. For large implementation work, use `engineering-workflow`; use `debugging-workflow` for evidence-based diagnosis; use `backend-architecture` for application and module boundaries; use `plain-english-explainer` for detailed teaching or explanations. These skills retain their own responsibilities.

## Foundational design principles

- Treat the database as the durable owner of data and invariants assigned to it. Use constraints and atomic operations where possible, not only application-side checks that can race.
- Choose relational modeling when relationships, joins, constraints, and multi-row transactions are central. Choose documents when data is commonly read and updated as a bounded aggregate and its shape fits the access patterns. Neither model is universally superior.
- Assign clear data ownership. In a modular monolith, modules can share one database while owning specific tables/collections and accessing other modules through intentional interfaces. In distributed systems, avoid multiple services independently writing the same records; define ownership and cross-service consistency explicitly.
- Normalize related facts when independent updates and integrity matter. Denormalize when measured access patterns justify copies and there is a clear update/reconciliation strategy.
- Name tables, columns, collections, fields, indexes, constraints, and migrations consistently with the repository. Prefer names that convey meaning and avoid schema organization by arbitrary technical layers.
- Keep database abstractions proportional. Use Prisma, Drizzle, Mongoose, or a native driver according to project conventions and the query needs; do not wrap them in generic layers that conceal transactions, query shape, or engine behavior.
- Optimize from evidence. Measure representative queries and workload before adding indexes, caching, partitioning, sharding, or changing engines. Indexes speed some reads but consume storage and add write and maintenance cost.
- Design for partial failure, retries, concurrency, and mixed application versions during deployment. Make retried writes idempotent where duplicate execution could change outcomes.
- Protect sensitive data throughout access, logs, backups, test fixtures, exports, and deletion workflows. Use synthetic or appropriately sanitized data outside production.

## Engineering rules

- Inspect schemas and migration history before changing them.
- Understand real query/access patterns before choosing indexes.
- Explain the model, constraints, transaction boundaries, and relevant trade-offs.
- Preserve existing data. Get explicit approval before destructive schema/data actions; production migrations and production data changes always require explicit authorization.
- Never expose credentials or sensitive records in source, logs, examples, or responses.
- Write tests at the useful database boundary: constraints, queries, transaction behavior, migration compatibility, and failure cases that mocks cannot prove.
- Verify migrations and important queries against a suitable test database when available; do not claim success if only reviewed statically.
- Avoid unnecessary abstraction, premature optimization, and introducing a database or cache without a requirement.
- Separate observed behavior from assumptions, proposed designs, and unverified production behavior.

## Integration and invocation

- “Use `$database-engineering` to design PostgreSQL tables and constraints for order checkout; explain transaction boundaries.”
- “Use `$database-engineering` with `$engineering-workflow` to add this migration safely and verify it against the test database.”
- “Use `$database-engineering` and `$backend-architecture` to decide which module owns these records and how other modules access them.”
- “Use `$debugging-workflow` to trace this slow query, then use `$database-engineering` to evaluate a fix from query-plan evidence.”
- “Use `$plain-english-explainer` to teach me why this migration needs an expand-and-contract rollout.”

## Scope and limitations

Version 1 has focused guidance for PostgreSQL, MySQL, MongoDB, and Redis. It provides reusable data-design and operational principles for other technologies, but their version-specific behavior should be checked against the engine's documentation and the project's actual configuration. This skill is not permission to access production data or credentials, and its design guidance does not substitute for a verified backup, restore, migration, or disaster-recovery exercise.

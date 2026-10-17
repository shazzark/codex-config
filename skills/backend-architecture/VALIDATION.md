# Scenario Validation

The scenarios below were used as structured design exercises against `SKILL.md` and its references. They evaluate guidance quality; they are not claims of generated or deployed applications, automated code tests, or production verification.

## 1. New Express + TypeScript REST API

**Scenario:** A small team starts a REST API for managing projects and tasks, with PostgreSQL as a likely future datastore but no initial need for workers or multiple services.

**Guidance outcome:** Start as a modular monolith with `projects` and `tasks` feature modules and a small startup/composition entrypoint. Keep route registration and request schemas close to each feature. A route handler can call a cohesive use case directly; add a store/repository only when persistence behavior or a testable boundary warrants it. Runtime-validate JSON, map expected errors centrally, and keep authorization/resource ownership explicit.

**Evaluation:** Appropriate feature boundaries without imposing a full clean-architecture tree. Dependencies flow from Express adapters toward use cases and persistence ports/adapters. No queue, Redis, or separate service is introduced without a requirement.

## 2. New Fastify + PostgreSQL backend

**Scenario:** A new API has customer and invoice capabilities, relational constraints, and a requirement to publish invoice events for asynchronous delivery.

**Guidance outcome:** Use Fastify plugins to encapsulate each capability and register shared dependencies before routes that need them. Keep schemas and route contracts visible, and place business invariants in use cases/domain functions rather than framework handlers. Use PostgreSQL constraints and migrations as durable truth; transact invoice state changes together. For reliable event publication, assess a transactional outbox and an idempotent worker rather than publishing before/after commit without recovery. Add indexes from expected query patterns and confirm plans with representative data.

**Evaluation:** Framework-specific boundaries are correct, database consistency drives transaction design, and async delivery introduces outbox/idempotency only because reliability is stated. No mandatory generic repository layer or microservice split.

## 3. Existing backend: small feature addition

**Scenario:** An existing Express API has routes and business logic in feature folders and no repository abstraction. Add an optional notification preference field to an account endpoint.

**Guidance outcome:** Inspect the current endpoint, validation, persistence, tests, and migration convention. Extend the established account module and its existing data path. Add a schema migration only if persistent storage requires it, keep default/backward compatibility explicit, and add focused validation and integration coverage. Do not reorganize folders or introduce a service/repository layer for the field unless the existing boundary has a demonstrated issue.

**Evaluation:** Established conventions are preserved; the feature is small and cohesive; no unrelated restructuring is justified. The engineering workflow can guide implementation and verification.

## Refinements made

The scenarios confirmed that the skill needed explicit exceptions against template-driven layering, service splitting, and broad refactors. The core skill now states proportionality and convention preservation up front. Stack guidance makes repositories conditional, keeps simple handlers direct, and ties queues, outboxes, transactions, and indexes to concrete consistency or workload needs.

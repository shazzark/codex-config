# Scenario Validation

The scenarios below were used as structured design exercises against `SKILL.md` and its references. They evaluate guidance quality; they are not claims of generated or deployed applications, automated code tests, or production verification.

## Original capability preservation review

Compared the enhanced skill with its pre-change source. Its activation scope and workflow remain intact. The enhancement retains the original proportional-architecture and established-convention rules, modular-monolith default, feature cohesion, optional layers/repositories, inward dependency direction, explicit module ownership, and Node.js/TypeScript implementation guidance for Express, Fastify, persistence, validation, authentication, Redis, queues, and workers. The existing quality/operations reference remains in place for API contracts, testing, observability, configuration, deployment, and review. Existing invocation examples and scope limitations remain unchanged. The only entrypoint addition routes system-level decisions to a new reference; the original implementation references were not rewritten.

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

## System-design scenario exercises

These six additional scenarios were reviewed as design exercises against the new system-design reference and the preserved implementation guidance. No application was built, benchmarked, load-tested, or deployed for these exercises.

### 1. Small CRUD application

**Guidance outcome:** Keep one deployable modular application with a few cohesive feature modules and the existing database. Do not add a queue, cache, dedicated service, or formal architecture layer without a requirement. Add validation, persistence constraints, and focused tests where useful.

**Evaluation:** Complexity remains proportionate; the system-design guidance does not override the original simple-CRUD and no-unnecessary-layer rules.

### 2. Growing modular monolith with increasing traffic

**Guidance outcome:** Establish representative latency, throughput, error, database-pool, and query evidence first. Address a measured bottleneck (for example, an inefficient query or bounded resource) before considering vertical capacity or stateless horizontal replicas. Add a cache or worker only when its workload and freshness/failure behavior justify it; keep module and data ownership explicit.

**Evaluation:** Scale options include their new connection, shared-state, consistency, and operational costs. Traffic growth alone does not mandate microservices.

### 3. Background processing with queues and workers

**Guidance outcome:** Use asynchronous processing when buffering, retry, decoupling, or separate resource scaling is a real requirement. Define message ownership/version, delivery and ordering expectations, bounded retry/queue behavior, idempotent effects, terminal failure handling, observability, and shutdown. Consider an outbox when database state and event publication must agree.

**Evaluation:** The design acknowledges duplicate delivery and cross-boundary consistency without claiming exactly-once effects or requiring a broker for all work.

### 4. Real-time application using WebSockets

**Guidance outcome:** Keep real-time transport as an adapter around application capabilities. Define authentication, message contracts, delivery/acknowledgement needs, bounded buffers, backpressure, reconnect behavior, and graceful drain. Scale connections or add cross-instance event distribution only when connection volume or topology requires it.

**Evaluation:** The architecture guide identifies the decisions but defers WebSocket, reverse-proxy, and networking mechanics to `$networking-engineering`; it does not prescribe a broker, sticky sessions, or distributed adapter automatically.

### 5. Multi-service system with database and network failures

**Guidance outcome:** Identify data owners and failure boundaries, use bounded operation deadlines and safe/idempotent retries, isolate unhealthy dependencies where justified, expose actionable health/latency/error signals, and define degraded behavior and recovery. Avoid a global transaction assumption across service and database boundaries.

**Evaluation:** The system-design reference treats partial failure and consistency as explicit choices, while detailed database, network, security, debugging, and operational procedures remain with their specialist skills. No diagnosis or runtime behavior is claimed without evidence.

### 6. Existing application considering microservices

**Guidance outcome:** Inspect current modules, deployment constraints, bottlenecks, ownership, and team operations. Compare keeping the modular monolith with a narrowly scoped extraction against the concrete need (independent scale, release, isolation, ownership, or failure containment). If extraction is justified, define one data owner and compatible contracts, plan migration/rollback, and adopt incrementally while observing behavior.

**Evaluation:** The skill does not recommend a rewrite or service split based only on growth or fashion. It requires a measurable or operational reason and includes the ongoing deployment, consistency, monitoring, security, and on-call costs.

## Validation scope and limitations

All six new scenarios and the original three scenarios are design exercises, not executable tests. The review checked whether recommendations follow stated requirements, preserve existing conventions, maintain dependency direction, and avoid unnecessary services, layers, or infrastructure. No runtime correctness, performance, availability, migration safety, or operational recovery was tested. The skill provides decision prompts; real estimates and choices still depend on product requirements, workload evidence, team capacity, and the target environment.

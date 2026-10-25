# Backend Project Organization

Use these patterns to choose a structure that fits the current requirements. They are options, not a mandatory tree. Inspect the repository first, follow its established conventions, and create only directories and files that have a clear role.

## Choose a proportionate shape

### Small API

For a simple service, utility API, or early prototype, keep a small number of direct, cohesive files. A route or handler can validate input, call a focused function, and use the persistence client directly when that remains clear. Add a startup/configuration module and tests where they solve a real need. Do not add controllers, services, repositories, interfaces, or a domain layer solely to satisfy a template; avoid abstractions that only pass arguments through.

### Feature-based modular backend

For a growing Express, Fastify, or similar backend, group code by feature or business capability so related behavior changes together. Add only the layers useful to that feature's complexity. For example:

```text
src/features/auth/
  auth.routes.ts
  auth.controller.ts
  auth.validation.ts
  auth.service.ts
  auth.repository.ts
  auth.test.ts
```

This is illustrative; a small feature might use only a route and a function, while a complex feature may use all or different files. Responsibilities:

- **Routes:** Declare transport paths/methods and connect framework hooks or middleware, validation, and handlers.
- **Controllers/handlers:** Translate transport input into an application call and its result into an HTTP response. In a small API, the route callback can do this directly.
- **Validation:** Runtime-check external data such as path, query, headers, and body, then expose trusted values to application code. TypeScript annotations alone do not validate input.
- **Services/use cases:** Coordinate a meaningful operation and enforce application policy without depending on framework request/response objects.
- **Repositories/stores:** Express persistence behavior when it creates a useful boundary or keeps queries and transaction intent cohesive. Do not hide useful database behavior behind generic CRUD.
- **Domain types/rules:** Represent business concepts and invariants when they have meaning independent of transport and storage. Do not invent a domain layer for straightforward CRUD without independent rules.
- **Tests:** Place focused unit or integration coverage near the feature or in the project's established test location, following existing conventions.

Keep transport concerns, business rules, and persistence concerns separate when they change for different reasons. Make cross-feature calls through explicit, narrow public exports rather than importing another feature's private files. Keep dependencies flowing in one clear direction; avoid circular module relationships. Shared code belongs in a shared module only when it has a genuine cross-feature owner.

### Larger backend systems

When requirements justify it, a modular monolith may also contain independently started API and worker processes, queue consumers/producers, shared packages, or distinct service boundaries. Keep capability and data ownership explicit. Extract a deployable service only for a concrete need such as independent scaling, isolation, release cadence, team ownership, security, or failure containment. Microservices are not the default response to growth.

Every extra process, queue, package boundary, or service adds deployment, configuration, observability, failure handling, data consistency, integration testing, and operational ownership costs. Include those costs in the decision and establish the tests and operational support the new boundaries require. See [System-design decisions](system-design-decisions.md) for service, scaling, reliability, and trade-off analysis.

## Directory responsibilities

These names describe common responsibilities, not a required global tree:

| Directory | Responsibility and placement |
| --- | --- |
| `config` | Validated runtime settings and configuration loading; keep secrets out of source and avoid scattered environment reads. |
| `routes` | Application-wide route composition or top-level route registration when framework conventions call for it. Feature route definitions usually belong beside their feature. |
| `controllers` | Transport adapters shared or grouped globally only when that matches the existing project. Otherwise keep controllers beside their feature; do not duplicate route handlers with pass-through wrappers. |
| `validation` | Shared schemas/policies only when reused across features. Request-specific schemas usually stay feature-local. |
| `features` | Business capability modules, each owning its related routes, application behavior, persistence access, and tests as needed. |
| `services` | Application-level operations only when there is a real shared or cross-feature service boundary. Avoid a global bucket that scatters feature behavior. |
| `repositories` | Shared persistence adapters only when their ownership and purpose are clear. Feature-specific queries usually stay with the feature that owns the data. |
| `middleware` | Express request/response pipeline functions or framework-neutral request policies when reusable. Keep feature-specific authorization or validation close to its route if that makes ownership clearer. |
| `plugins` | Fastify plugins and other framework extensions that register capabilities, decorators, hooks, or encapsulated routes. Feature plugins may live under their feature. |
| `domain` | Cross-feature domain concepts only when truly shared and governed by an explicit owner. Prefer feature-local domain rules otherwise. |
| `shared` | Small, cohesive code used by multiple features and not owned by one feature; avoid turning it into a dumping ground. |
| `utils` | Generic, side-effect-light helpers with broad reuse. Feature-specific helpers stay with their feature. |
| `types` | Shared types that cannot live naturally beside their owner; do not centralize every type. Types do not replace runtime validation. |
| `tests` | Global integration, contract, or end-to-end tests when that matches the project. Unit tests may live beside the code; retain a consistent test discovery convention. |

A global directory is useful when it provides a genuinely shared policy, adapter, or framework composition point with clear ownership. If its contents serve one capability, keep them feature-local. Do not combine transport handling, business rules, persistence, configuration, and generic utilities in one catch-all module when their responsibilities or change patterns differ.

## Express and Fastify conventions

- **Express:** Middleware operates in the request/response chain and is generally composed at application or router setup. Keep shared middleware in a clear shared location and feature-specific route wiring with the feature. Order parsing, authentication, route authorization/validation, handlers, not-found handling, and centralized error handling deliberately. Express middleware is not the same concept as Fastify plugins or hooks.
- **Fastify:** Plugins provide encapsulation for routes, decorators, hooks, and schemas. Register dependencies before the route plugins that use them; use feature plugins to keep capability boundaries clear. Hooks run at defined request lifecycle points. Use plugins/hooks according to Fastify's lifecycle and encapsulation rather than creating an Express-style global middleware tree.
- **Both:** Keep business operations independent of framework request/response objects. Do not call React hooks backend hooks: React hooks are frontend component logic. For runtime error, validation, authentication, and framework lifecycle details, see [Node.js and TypeScript patterns](node-typescript.md).

## Full-stack boundary

The backend owns server-side APIs, authorization, business rules, persistence, and background work. The frontend owns UI components, React hooks and context, page/route rendering, client-side services, and client state management. Keep these concerns in their respective application areas; a shared repository does not make them one layer.

Frontend feature folders may consume backend APIs through a client/service and typed contracts. Share API schemas or generated contracts only when there is actual reuse and a compatible build/release model; keep runtime validation at the server boundary regardless. Do not move UI state or React hooks into backend modules. For application folders and React/Next.js boundaries, consult [frontend application architecture](../../frontend-experience/references/application-architecture.md); for visual direction and foundations, consult [frontend-experience](../../frontend-experience/SKILL.md) and [design-system-engineering](../../design-system-engineering/SKILL.md).

## Architecture selection

For each task:

1. Inspect the repository structure, instructions, dependencies, feature boundaries, and relevant tests.
2. Understand the application size, expected change, runtime constraints, and concrete quality needs; keep unknowns explicit.
3. Prefer established conventions that already support the change.
4. Choose the simplest of the small API, feature-based modular backend, or larger-system shapes that meets current requirements; explain meaningful trade-offs when options differ.
5. Create only necessary files and directories. For a new project, recommend a shape from its requirements. For an existing project, preserve its architecture unless restructuring was explicitly requested and justified.
6. Preserve public APIs and existing behavior during any justified restructuring; avoid unrelated moves or renames.
7. For substantial architecture changes, use focused tests at affected boundaries and review the diff against the intended scope. Follow `engineering-workflow` for substantial execution and `testing-engineering` for test selection.

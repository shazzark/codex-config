# Document template guide

Templates are optional starting points. Copy only the relevant template into the project, rename it to the chosen project convention, remove unused optional sections, and link related documents. Do not create every file just because a mode lists it as possible.

Every template states its purpose, when it is useful, required and optional sections, how to mark confirmed information versus assumptions, how to write acceptance criteria, and which documents it relates to. Keep documents as `Draft` until approved. See [planning modes and discovery](planning-modes.md) for mode selection and workflow.

## Select by question

| Document | Use it to answer |
|---|---|
| [`project-brief.md`](templates/project-brief.md) | Why are we doing this, for whom, and what outcome matters? |
| [`mvp-scope.md`](templates/mvp-scope.md) | What is the smallest useful release, and what is explicitly out? |
| [`prd.md`](templates/prd.md) | What product behavior and business rules must users be able to rely on? |
| [`trd.md`](templates/trd.md) | Which testable technical requirements, constraints, and dependencies must the solution meet? |
| [`appflow.md`](templates/appflow.md) | How does a user move through primary journeys and their states? |
| [`architecture.md`](templates/architecture.md) | What are the system boundaries, major components, and design choices? |
| [`database-schema.md`](templates/database-schema.md) | What data concepts, ownership, relationships, and integrity rules are needed? |
| [`design-system.md`](templates/design-system.md) | What visual foundations, components, and accessibility rules should be shared? |
| [`external-integrations.md`](templates/external-integrations.md) | How will an external provider or tool be used and what happens when it fails? |
| [`risk-register.md`](templates/risk-register.md) | Which uncertain events could materially affect users, delivery, or operations? |
| [`testing-acceptance-plan.md`](templates/testing-acceptance-plan.md) | What evidence is needed to accept product behavior and technical quality? |
| [`architecture-decision-record.md`](templates/architecture-decision-record.md) | Why was one material technical option chosen over alternatives? |
| [`implementation-plan.md`](templates/implementation-plan.md) | In what bounded phases will approved scope be delivered and verified? |
| [`tasks.md`](templates/tasks.md) | What small, executable work items make up an approved phase? |

## Keep ownership clear

- `project-brief.md` gives the concise purpose and context; `mvp-scope.md` limits the first release.
- `prd.md` owns product behavior and business requirements. `trd.md` owns testable technical requirements, constraints, and dependencies. For a small project, combine technical notes into a shorter specification rather than creating a separate TRD.
- `architecture.md` owns system structure, boundaries, and the current design. An ADR records one consequential choice and its history; it should link to the architecture and not duplicate it.
- `implementation-plan.md` owns phase sequencing, deliverables, dependencies, risks, and completion checks. `tasks.md` owns executable work items linked to phases; task lists do not replace requirements.
- Specialist skills own detailed designs: backend structure, database schemas/operations, security/authentication, design systems, network/caching behavior, tests, and deployment. Planning documents state the needs and constraints, then link to the specialist work rather than copying it wholesale.

Acceptance criteria should be observable and testable. State the actor/context, action, and expected result; include a measurable threshold only when its target and measurement method are agreed. Link each important criterion to its source requirement and planned verification. Do not mark stakeholder validation or technical feasibility as complete without evidence.

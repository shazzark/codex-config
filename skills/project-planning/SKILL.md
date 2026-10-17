---
name: project-planning
description: Turn software ideas or existing-project requests into clear requirements, a proportionate MVP, selected planning documents, and an actionable delivery plan before implementation.
---

# Project Planning

Use this skill to clarify what a software project should do and prepare an approved, buildable plan. It supports web and mobile apps, APIs, CLI tools, AI products, SaaS, multi-service systems, and expansions to existing projects. It owns discovery, MVP scope, document selection, and the handoff to implementation; it does not implement the application.

## Planning workflow

1. Restate the problem, target users, and intended outcome. Inspect existing project documentation and conventions when planning an expansion.
2. Choose Quick, Standard, or Advanced depth based on actual complexity and risk. Read [planning modes and discovery](references/planning-modes.md).
3. Ask focused questions about important unknowns. Separate confirmed facts, assumptions, recommendations, and open questions. Never present invented business rules, user preferences, provider permissions, or API capabilities as facts.
4. Define the smallest useful MVP, explicit non-goals, user journeys, business rules, dependencies, constraints, risks, and measurable acceptance criteria.
5. Recommend a technology approach with practical trade-offs. Use specialist skills for detailed architecture, data, security, design, testing, networking, caching, and operations as relevant; keep their responsibilities in those skills.
6. Propose only the documents needed. Use [document templates](references/template-guide.md) and the individual templates under `references/templates/`. Keep each concern in its owning document and link related documents instead of copying whole sections.
7. Present major product and technical decisions for approval before marking requirements or plans approved. Documents may be prepared as drafts when requested; label unapproved assumptions and decisions clearly.
8. Turn approved scope into bounded phases with objectives, scope, dependencies, deliverables, acceptance criteria, verification, risks/blockers, and completion conditions.
9. Stop at the planning handoff. Do not write application code until the user approves implementation. Do not create remote repositories, install dependencies, configure cloud services, deploy, or otherwise act outside the planning work unless separately authorized.

## Document boundaries

- `project-brief.md`: problem, users, outcome, and constraints.
- `mvp-scope.md`: prioritized MVP, non-goals, and later work.
- `prd.md`: product behavior, business rules, journeys, and product acceptance.
- `trd.md`: testable technical requirements, constraints, dependencies, and technical acceptance. For small projects, use a short technical-requirements section in a combined specification instead of a separate TRD.
- `architecture.md`: system structure, boundaries, and design decisions; detailed domain choices remain with specialist skills.
- `implementation-plan.md`: phased delivery and dependencies; `tasks.md` breaks approved phases into executable work items.

## Work with related skills

- `$engineering-workflow` owns the approved implementation process, repository checks, documentation synchronization, and checkpoints. This skill prepares the plan it can consume.
- `$backend-architecture` owns detailed backend organization and system-design trade-offs.
- `$language-engineering` owns language-specific implementation choices.
- `$database-engineering` owns database selection, models, queries, migrations, and operations.
- `$authentication-engineering` owns identity and access-control design; `$security-engineering` owns broader threat analysis and controls.
- `$design-system-engineering` owns interface foundations, component behavior, accessibility, and Figma-to-code decisions.
- `$networking-engineering` owns communication paths and network-level choices; `$caching-engineering` owns cache correctness and strategy.
- `$testing-engineering` owns detailed test design and execution; this skill records product acceptance and a proportionate test strategy.
- `$devops-infrastructure` owns deployment and operational design; `$debugging-workflow` owns investigation of observed defects.
- `$plain-english-explainer` guides clear explanations; this skill already asks for everyday language and defined technical terms.

## Invocation examples

- "Use `$project-planning` in Quick mode to turn this CLI idea into an MVP brief and a small implementation plan. Keep unknowns labeled and stop before coding."
- "Use `$project-planning` in Standard mode to plan a mobile application. Ask only the questions that affect its MVP, then propose the necessary documents for approval."
- "Use `$project-planning` in Advanced mode for this AI agent with external tools. Include a TRD, integration boundaries, security/privacy requirements, risks, and testable acceptance criteria."
- "Use `$project-planning` to plan this feature in the existing app. Inspect its current docs and structure first, preserve conventions, and create draft updates only."

## Limits

A plan is not stakeholder approval, user validation, a feasibility proof, or a guarantee of delivery cost/date. Estimates and provider capabilities require evidence from the actual team, contracts, and target environment. Keep draft status until the appropriate decision-maker approves; report unresolved decisions rather than guessing.

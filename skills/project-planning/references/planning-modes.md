# Planning modes and discovery

Choose depth by the project's uncertainty, risk, scope, and operational needs—not by ambition or technology labels. The modes set a ceiling on ceremony, not a checklist to complete.

## Quick mode

Use for a small CLI tool, utility, prototype, or low-risk project with few users, integrations, and operational constraints.

Usually enough:

- `project-brief.md` for the problem, user, desired outcome, and constraints.
- `mvp-scope.md` for the smallest useful feature set and non-goals.
- `implementation-plan.md` for a few ordered phases and completion checks.

Combine these into one short specification if that is clearer. Add a PRD, TRD, architecture, database, design-system, or test plan only when a real decision needs it.

## Standard mode

Use for a typical web/mobile application or backend platform with multiple journeys, persistent data, user roles, or integrations. Select only relevant documents from the template guide. Common choices include project brief, MVP scope, PRD, app flow, architecture, implementation plan, and tasks. Add `trd.md` when technical constraints, dependencies, integrations, security, performance, reliability, or operations need explicit testable requirements. Add database and design-system documents when the project has meaningful data-model or UI-system decisions.

## Advanced mode

Use when real complexity warrants coordinating system boundaries, several services/teams, sensitive data, AI agents or tools, significant reliability/performance targets, external providers, or deployment/recovery constraints. Include relevant Standard documents plus a TRD and only the needed supporting documents: architecture decisions, external integrations, risk register, and testing/acceptance strategy. Capture milestones and dependencies in the implementation plan. Advanced mode does not automatically mean microservices, multiple databases, cloud infrastructure, or a long document set.

## Discovery sequence

### Understand the problem

Restate the user problem in one or two sentences. Identify who experiences it, what they do today, and the outcome the software should improve. Do not turn a proposed solution (“build an app with AI”) into a confirmed need without checking the underlying problem.

### Clarify users and journeys

Identify primary user groups, their key goals, and the main end-to-end journeys. Ask which journey must work first. Capture business rules, permissions, error/empty states, and data ownership only when known; label unresolved rules.

### Ask only material questions

Ask concise questions whose answers could change MVP scope, a major product decision, safety/privacy, platform choice, feasibility, or cost. Group related unknowns and offer a reasonable recommendation where useful, but mark it as a proposal. Do not repeatedly ask for details that can be recorded as a reversible assumption. Proceed with unaffected planning while waiting for answers.

Common question areas, when relevant:

- Who are the initial users, and what outcome makes the first release useful?
- Which platforms and accessibility needs are required at launch?
- Which data is sensitive, who may access it, and what retention/deletion rules apply?
- Which external systems, providers, credentials, contracts, and permissions are available?
- What workload, latency, availability, recovery, or offline behavior is actually required?
- What team skills, budget, deadline, existing system, or hosting constraints are fixed?
- For AI features: what tools/data may the model access, which actions need human confirmation, how is output evaluated, and what cost/latency/privacy limits apply?

### Separate certainty levels

Use explicit labels such as `Confirmed`, `Assumption`, `Recommendation`, and `Open question`. Include the source or decision owner when it matters. A recommendation is not approved just because it appears in a draft. Do not invent provider capabilities, legal obligations, user preferences, traffic targets, or business rules.

### Define the MVP and success

Prioritize by user value, risk reduction, dependencies, and effort. State must-have behavior, intentionally excluded work, and later options separately. Define a small number of observable success measures and acceptance criteria. A proposed metric is not a validated target until the owner agrees; an MVP is not user-validated until representative users or stakeholders actually evaluate it.

### Plan and seek approval

Recommend the mode and document set with a reason for each document. Put significant choices (product behavior, target platform, data handling, provider, architecture constraints, budget/availability targets) in an approval list. Keep documents in `Draft` until accepted by the user/stakeholder. Planning approval does not authorize code changes, dependency installation, repository creation, cloud configuration, or deployment.

## Existing-project expansion

Inspect the current README, requirements, architecture notes, task/issue context, relevant source boundaries, and established conventions before proposing new documents or changes. Treat current behavior and deployed constraints as evidence, not as desired behavior. Update existing documents where appropriate instead of creating competing versions. Keep new requirements separate from implementation observations and avoid broad restructuring as a planning side effect.

## Planning handoff

An actionable handoff includes approved scope, unresolved questions, document links, phase order, dependencies, deliverables, acceptance/verification criteria, risks, and explicit completion conditions. `engineering-workflow` takes over when the user approves implementation. Update plans when scope changes; preserve decision history where it helps explain why the plan changed.

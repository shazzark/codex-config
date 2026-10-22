---
name: engineering-workflow
description: Apply a disciplined software-engineering workflow to substantial implementation, feature, refactor, or multi-phase repository work. Use when work benefits from repository inspection, planning, testing, review, documentation synchronization, and safe Git checkpoints. Do not activate for trivial edits or simple questions.
---

# Engineering Workflow

Use this skill to execute substantial repository work safely and incrementally.

Preserve the user's requested scope, architecture, product decisions, and authorization boundaries. Repository-specific instructions and approved project documents take precedence over generic workflow preferences in this skill.

## Establish the current state

Before substantial implementation:

1. Inspect the repository and relevant existing code.
2. Read applicable repository instructions such as `AGENTS.md`.
3. Read relevant planning documents when present, such as `prd.md`, `architecture.md`, and `task.md`.
4. Inspect `git status`, relevant diffs, and recent history when Git state matters.
5. Identify existing work before creating replacements.

Do not assume an incomplete-looking working tree is disposable.

## Plan proportionally

For non-trivial work, determine:

- the objective,
- observable completion criteria derived from the user's actual requirements,
- relevant existing architecture,
- dependencies and constraints,
- affected components,
- verification strategy,
- meaningful risks.

Keep planning proportional to the task. Do not create ceremony for small changes.
Formal planning documents are not required for small changes.

When `task.md` or another execution plan exists, use it as the current execution-state source unless repository instructions say otherwise.

## Implement incrementally

Make the smallest coherent set of changes that completes the current objective.

Preserve established architecture unless changing it is part of the approved task.

Use specialist subagents when independent investigation, domain expertise, parallel research, or review would materially improve the result. Do not delegate merely to satisfy a process.

The lead agent owns integration and repository-level decisions unless project instructions explicitly establish another model.

## Test while implementing

Match verification depth to the change's risk and the evidence needed to establish its completion criteria. Use meaningful tests when they establish relevant behavior; avoid unnecessary tests and redundant checks. Do not impose test-first development on every task.

Before declaring a substantial unit of work complete, run the repository's applicable verification gates. These may include:

- tests,
- type checking,
- linting,
- build,
- integration tests,
- package or consumer verification,
- other project-specific checks.

Never claim a check passed unless it actually ran successfully.

Report which checks passed, what behavior remains untested, and any blockers. Distinguish verified results from assumptions.

Do not weaken tests, types, validation, or quality gates merely to obtain a passing result.

## Review before completion

Review the implementation against:

- the requested behavior,
- existing architecture,
- edge cases,
- error handling,
- tests,
- security or correctness risks relevant to the change.

For high-risk or technically sensitive changes, use an independent review when available and useful.

Resolve material findings before checkpointing.

## Inspect the final diff

Before creating a Git checkpoint, inspect the working tree and relevant diff.

Check for:

- accidental unrelated changes,
- generated artifacts that should not be committed,
- debug code,
- secrets,
- incomplete work,
- stale documentation,
- unintended dependency changes.

## Synchronize project state

When the repository maintains execution or architecture documentation, update it to reflect reality.

Prefer:

- `prd.md` for product intent and scope,
- `architecture.md` for technical design and important contracts,
- `task.md` for execution status and next work,
- `AGENTS.md` for repository-specific agent instructions.

Do not mechanically create these files in repositories that do not need them.

Documentation must describe what actually exists, not planned work as though it were complete.

## Create safe checkpoints

When Git is available and a substantial logical unit is complete and verified, create a focused checkpoint commit if repository instructions and user authorization permit commits.

A checkpoint should represent a coherent, working state.

Do not create commits merely because files changed.

Do not automatically push every checkpoint. Push only when requested, authorized by the project workflow, or required by the approved release process.

Never force-push, rewrite shared history, or perform destructive Git operations without explicit justification and authorization.

For detailed Git safety and crash recovery behavior, read:

- [Git safety](references/git-safety.md)
- [Recovery workflow](references/recovery.md)

## Continue deliberately

For multi-phase work, after a verified checkpoint:

1. update execution state,
2. identify the next incomplete approved objective,
3. continue when the user's authorization covers continued execution.

Do not expand into future, optional, or explicitly deferred scope simply because earlier work is complete.

Stop when external credentials, destructive actions, unresolved product decisions, permission boundaries, or other genuine blockers require user input.

## Core operating loop

Use this as a guide, not ceremony:

`inspect -> understand -> plan -> implement/delegate -> test -> review -> fix -> verify -> diff review -> document -> checkpoint -> continue`

Adapt the depth of each step to the risk and complexity of the task.

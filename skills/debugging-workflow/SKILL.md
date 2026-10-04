---
name: debugging-workflow
description: Systematically investigate and fix software bugs, failing tests, runtime errors, broken integrations, incorrect behavior, performance regressions, and development-environment failures. Use when the primary task is diagnosing why something is broken. Prefer evidence-driven root-cause analysis, minimal fixes, regression protection, and verification over speculative edits.
---

# Debugging Workflow

Use this skill when the primary objective is to diagnose and fix something that is not behaving as expected.

The goal is not merely to make the visible error disappear. The goal is to identify the root cause, implement the smallest correct fix, and prove that the problem is resolved without introducing regressions.

Repository-specific instructions and user constraints take precedence over this generic workflow.

## Establish the symptom

Start by determining exactly what is failing.

Identify:

- expected behavior,
- actual behavior,
- error messages or stack traces,
- affected environment,
- relevant inputs,
- whether the failure is consistent or intermittent,
- the smallest known reproduction.

Do not assume the visible symptom identifies the root cause.

## Reproduce before changing code

When practical, reproduce the failure before implementing a fix.

Use the narrowest useful reproduction:

- failing test,
- API request,
- browser interaction,
- CLI command,
- build command,
- minimal input,
- isolated component or function.

Record useful evidence from the reproduction.

If reproduction is impossible, explain why and continue using the strongest available evidence.

Do not fabricate a reproduction.

## Inspect before hypothesizing

Inspect the relevant execution path and evidence.

Depending on the problem, inspect:

- logs,
- stack traces,
- network requests and responses,
- application state,
- environment/configuration,
- database interactions,
- external service responses,
- recent code changes,
- relevant tests,
- dependency/runtime versions.

Trace the failure across boundaries when necessary.

Examples:

`UI -> client state -> API request -> route -> controller/service -> database`

or:

`command -> configuration -> runtime -> dependency -> operating system`

Do not randomly modify files while searching for the cause.

## Form and test hypotheses

Based on evidence, identify plausible causes.

Prefer hypotheses that explain all important observed symptoms.

Test hypotheses individually when practical.

Use experiments that distinguish between competing explanations.

Avoid changing several unrelated things at once because doing so makes it difficult to know what actually fixed the problem.

Distinguish clearly between:

- observed fact,
- hypothesis,
- confirmed root cause.

For deeper investigation guidance, read:

- [Root cause analysis](references/root-cause-analysis.md)

## Identify the root cause

Before implementing a substantial fix, establish why the failure occurs.

A root cause should explain the failure mechanism, not merely restate the symptom.

Example:

Weak conclusion:

`Login spinner is stuck.`

Better conclusion:

`The authentication bootstrap waits indefinitely because the /users/me request never settles after the database connection fails.`

Do not claim certainty beyond the available evidence.

## Implement the smallest correct fix

Fix the root cause rather than hiding the symptom.

Prefer a focused change that preserves existing architecture and behavior.

Do not:

- swallow errors merely to remove an error message,
- disable validation to make invalid input pass,
- weaken types to silence type errors,
- remove failing tests simply to obtain a green test suite,
- add arbitrary delays or retries without evidence,
- perform unrelated refactors during a focused bug fix.

If investigation reveals that a larger architectural change is genuinely necessary, explain the reason and scope before expanding substantially.

## Add regression protection

When practical, add or update a test that would have failed before the fix and passes afterward.

The regression test should protect the behavior that actually broke.

For detailed verification guidance, read:

- [Regression verification](references/regression-verification.md)

## Verify the fix

After implementation:

1. rerun the original reproduction,
2. run focused tests for the affected behavior,
3. run relevant surrounding tests,
4. run applicable type checking, linting, build, or integration checks when warranted,
5. inspect for side effects.

Never claim the bug is fixed solely because the code looks correct.

Never claim a verification step passed unless it actually ran successfully.

## Review the final change

Review the final diff and confirm:

- the root cause is addressed,
- the symptom is resolved,
- unrelated behavior was not intentionally changed,
- temporary debugging code is removed,
- logs or instrumentation added only for investigation are removed unless intentionally retained,
- tests meaningfully protect the fix,
- documentation is updated when the bug exposed an important operational or architectural fact.

## Preserve repository safety

When debugging inside a Git repository, preserve unknown existing work.

Do not use destructive Git cleanup as a debugging technique.

When the `engineering-workflow` skill is available, follow its Git safety and checkpoint principles for substantial repository changes.

A completed bug fix may become a focused checkpoint after verification.

## Debugging loop

Use this as a guide rather than rigid ceremony:

`observe -> reproduce -> inspect -> trace -> hypothesize -> test hypothesis -> identify root cause -> fix -> regression test -> verify -> review`

Repeat earlier stages when new evidence disproves the current hypothesis.

## Stop conditions

Ask for user input when progress genuinely requires:

- credentials or access not available,
- destructive actions,
- unavailable external systems,
- unresolved product behavior,
- hardware or environment access that cannot be inspected,
- a decision that materially changes product scope.

Do not stop merely because the first hypothesis was wrong. Update the hypothesis and continue investigating.

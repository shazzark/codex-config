# Root Cause Analysis

Use this reference when the cause of a software failure is uncertain, spans multiple components, or cannot be established from the initial error alone.

## Separate symptom from cause

A symptom describes what the user or system observes.

Examples:

- login spinner never finishes,
- API returns 500,
- page is blank,
- test times out,
- database query fails,
- realtime update does not appear.

The root cause explains the mechanism that produces that symptom.

Do not stop investigation merely because the visible symptom has been located.

## Build an evidence chain

Collect evidence that narrows where the failure occurs.

Useful evidence may include:

- exact error messages,
- stack traces,
- logs,
- HTTP status codes,
- request and response payloads,
- timing information,
- application state,
- database results,
- environment variables and configuration presence,
- dependency/runtime versions,
- recent Git changes,
- failing and passing tests.

Prefer direct evidence over assumptions.

## Trace boundaries

When a feature crosses multiple layers, trace the path in execution order.

For a web application this may be:

`user action -> UI handler -> client state -> request -> API route -> controller/service -> database/external service -> response -> client update -> rendered UI`

At each boundary ask:

1. Did execution reach this point?
2. Was the input correct?
3. Did the operation complete?
4. Was the output correct?
5. Was the output passed correctly to the next layer?

The first broken boundary often sharply narrows the investigation.

## Compare working and failing paths

When possible, compare the failure with something similar that works.

Compare:

- inputs,
- configuration,
- code paths,
- environment,
- permissions,
- database state,
- request payloads,
- dependency versions,
- timing or concurrency behavior.

A controlled difference can reveal the cause faster than broad code inspection.

## Use hypotheses explicitly

For each plausible explanation, identify:

- the hypothesis,
- evidence supporting it,
- evidence against it,
- an experiment that could confirm or reject it.

Prefer experiments that eliminate multiple possibilities.

Do not treat correlation as proof.

## Narrow the problem

Reduce the failing system when practical.

Examples:

- reproduce an API failure without the UI,
- test a service without the route layer,
- test a function with a minimal input,
- replace an external dependency with a controlled test double,
- reproduce a database operation directly,
- isolate a configuration value,
- run one failing test instead of the entire suite.

The objective is to find the smallest boundary where expected behavior becomes incorrect.

## Check recent changes carefully

Recent changes are useful clues but are not automatically the cause.

Use Git history and diffs to identify:

- changed contracts,
- renamed fields,
- dependency updates,
- configuration changes,
- altered validation,
- changed async behavior,
- schema changes.

Verify causality before reverting or modifying them.

## Consider environment failures

Not every software failure is an application-code bug.

Possible causes include:

- missing environment variables,
- unavailable database,
- expired credentials,
- DNS/network failures,
- port conflicts,
- incompatible runtime versions,
- stale processes,
- incorrect deployment configuration,
- unavailable external services.

Do not rewrite correct application code to compensate for an unrelated environment failure.

## Consider async and state failures

For hangs, spinners, stale UI, realtime bugs, or intermittent failures, inspect:

- unresolved promises,
- missing timeout/error paths,
- race conditions,
- stale closures or state,
- duplicate subscriptions,
- missing cleanup,
- event ordering,
- retry behavior,
- connection lifecycle.

Use evidence to establish which condition actually occurs.

## Confirm the root cause

A strong root-cause conclusion should explain:

- why the observed symptom occurs,
- why it occurs under the failing conditions,
- why the proposed fix prevents it,
- and, when relevant, why the behavior previously worked or only fails intermittently.

If those questions cannot yet be answered, continue investigating or state the remaining uncertainty.

## Avoid common debugging traps

Do not:

- make speculative edits before gathering evidence,
- change many variables simultaneously,
- confuse the first error observed with the underlying cause,
- repeatedly add retries or delays without understanding the failure,
- blame dependencies without evidence,
- assume configuration is correct because it worked previously,
- assume code is correct because tests passed in another environment,
- stop at a workaround when the root cause remains active.

The purpose of root-cause analysis is not to produce the most complicated explanation.

It is to produce the simplest explanation that is supported by the evidence and accounts for the failure.

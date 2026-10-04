# Regression Verification

Use this reference after the root cause has been identified and a fix is being implemented or reviewed.

The objective is to prove that the original failure is fixed and that the change does not introduce a meaningful regression.

## Reproduce before and after

When practical, preserve a reproducible case for the original bug.

A strong debugging sequence is:

1. demonstrate the failure,
2. implement the fix,
3. run the same reproduction again,
4. confirm the expected behavior.

This creates evidence that the change actually addresses the reported problem.

## Add regression tests

When practical, create or update an automated test representing the broken behavior.

A useful regression test should:

- fail against the broken behavior,
- pass with the fix,
- test the relevant contract rather than implementation trivia,
- remain useful after the debugging session ends.

Choose the appropriate level:

- unit test,
- integration test,
- API test,
- component test,
- end-to-end test.

Do not add a test solely to increase test count.

## Verify surrounding behavior

A focused fix may affect neighboring behavior.

After the targeted test passes, run relevant surrounding tests.

Examples:

- authentication fix -> auth/session/protected-route tests,
- API validation fix -> valid and invalid request tests,
- database fix -> relevant integration tests,
- realtime fix -> connection/subscription/update tests,
- package fix -> package tests plus consumer/import verification.

Expand verification according to the blast radius of the change.

## Test important failure paths

Do not verify only the happy path when the bug involves errors or edge cases.

Consider relevant cases such as:

- invalid input,
- unavailable dependency,
- timeout,
- missing configuration,
- unauthorized request,
- empty data,
- duplicate operation,
- retry,
- cancellation,
- concurrent execution.

Only test cases relevant to the actual change.

## Run project quality gates

When warranted by the repository and scope, run:

- tests,
- type checking,
- linting,
- build,
- integration checks,
- package checks,
- end-to-end checks.

Do not substitute one quality gate for another when they protect different failure classes.

## Check the original symptom directly

Automated tests are valuable, but when the reported bug is user-visible or environment-specific, verify the original symptom directly when practical.

Examples:

- confirm the spinner now resolves,
- confirm the request returns,
- confirm the page renders,
- confirm the realtime event appears,
- confirm the command completes,
- confirm the production build starts correctly.

## Inspect for accidental masking

Ensure the fix did not merely hide the failure.

Warning signs include:

- empty catch blocks,
- ignored rejected promises,
- suppressed error messages,
- disabled validation,
- removed assertions,
- skipped tests,
- arbitrary fallback values,
- unconditional success responses,
- excessive retries masking a persistent failure.

A successful-looking UI is not proof that the underlying operation succeeded.

## Review the blast radius

Ask:

- What code paths changed?
- What contracts depend on them?
- Could existing callers behave differently?
- Did data shape change?
- Did error behavior change?
- Did timing or async behavior change?
- Did configuration requirements change?

Use the answers to determine additional verification.

## Remove temporary debugging artifacts

Before completion, remove temporary artifacts unless intentionally retained:

- console logging,
- temporary endpoints,
- test-only bypasses,
- hardcoded credentials or values,
- diagnostic UI,
- temporary sleeps,
- commented-out production code.

Keep useful permanent observability only when it belongs in the product.

## Completion criteria

A bug fix is ready to be considered complete when there is sufficient evidence that:

1. the root cause is understood,
2. the fix addresses that cause,
3. the original reproduction succeeds,
4. relevant regression protection exists when practical,
5. surrounding behavior still works,
6. applicable quality gates pass,
7. temporary debugging artifacts are removed,
8. the final diff contains only intentional changes.

Report any verification that could not be performed and why.

Never convert an unverified assumption into a claim that the bug is fixed.

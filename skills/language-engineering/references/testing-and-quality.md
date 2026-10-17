# Testing and quality

## Match tests to behavior

- **Unit tests** cover a focused function or module and its important decisions, edge cases, and errors.
- **Integration tests** check behavior across real module or runtime boundaries, such as file access, database adapters, network clients, or package exports.
- Add regression tests for fixed defects and negative-path tests for invalid, missing, malformed, and failure inputs.
- Use contract tests when separately developed modules or services rely on a shared language-level/API contract.
- Avoid tests that only repeat implementation details or assert that a helper was called when the observable behavior matters more.

Use a suitable isolated environment for tests that touch databases, files, queues, or external systems. A mock can check caller decisions but cannot establish that the real runtime, driver, or external contract behaves as expected.

## Quality checks

Run checks that the project actually defines and that fit the changed code:

- compiler/type checker (`tsc`, project build, or language-specific equivalent);
- unit/integration tests;
- linting and formatting;
- static analysis or dependency checks where configured;
- package/build checks for public libraries and supported module formats.

Inspect existing scripts and CI before inventing a new command or adding a tool. Do not reformat unrelated files. Fix relevant findings without suppressing useful diagnostics just to make the command pass.

## Performance and coverage claims

Benchmark only when there is a credible performance question, use representative input and runtime conditions, and compare equivalent builds. A microbenchmark may not represent application behavior. Avoid optimizing based only on intuition.

Coverage is a measure of which code ran, not whether assertions prove correctness. Report test names/commands and results accurately. State when tests were not run and distinguish static review, design exercise, mock-based test, and real integration verification.

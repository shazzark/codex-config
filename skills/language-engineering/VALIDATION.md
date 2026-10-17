# Language-engineering scenario review

These five reviews are design exercises for checking the instructions. They are not execution of a sample codebase, compiler runs, or test results.

| Scenario | Appropriate guidance | Check type and result |
|---|---|---|
| Refactor JavaScript module to strict TypeScript | Inspect behavior, runtime/config, module format, and tests; migrate incrementally; use `unknown` and runtime parsing at boundaries; avoid unsafe casts; verify typecheck and existing tests. | Design exercise; no source module or compiler run supplied. |
| Design reusable TypeScript library API | Keep the exported surface small and stable; use generics only to preserve useful input/output relationships; document runtime validation and package `exports`/module compatibility; test supported consumer formats. | Design exercise; no library build or consumer test. |
| Fix asynchronous JavaScript error handling | Reproduce first with debugging workflow; identify who owns each promise; await/return it or deliberately collect outcomes; handle cleanup and rejection without detached work; add regression coverage. | Design exercise; no failing code or test executed. |
| Review TypeScript backend for type-safety problems | Look for external values asserted instead of parsed, `any`, non-null assertions, unsafe error narrowing, nullable-state mistakes, floating promises, and inconsistent contracts. Route module boundaries and database/security design to their owning skills. | Design exercise; no backend inspected or checks run. |
| Decide whether an abstraction or dependency is justified | Check repeated concrete use, maintenance cost, project conventions, runtime compatibility, and simpler alternatives. Add only when it reduces real duplication or complexity; verify lockfile/build/tests if added. | Design exercise; no dependency or benchmark evaluated. |

## Findings and limits

The exercises keep language-level decisions distinct from backend, database, security, authentication, caching, and UI design responsibilities. They prompt runtime validation in addition to static typing, and make asynchronous failure ownership visible. These reviews do not verify compiler behavior in a project or prove that code is correct; actual repository configuration, implementation, and tests are required.

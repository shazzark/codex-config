# Review Checklist

Select only the dimensions affected by the change. Follow project requirements, established conventions, and relevant specialist skills for detailed criteria.

## Scope and requirements

- Compare completed work with approved requirements and acceptance criteria, including explicit exclusions and dependencies.
- Check that the diff matches the stated scope and that relevant callers, interfaces, and surrounding code were inspected.
- Treat missing, conflicting, or unapproved requirements as an uncertainty to report, not as permission to invent expected behavior.

## Possible review dimensions

- **Correctness:** control flow, boundary conditions, error handling, state transitions, and regressions.
- **Interfaces and compatibility:** API contracts, callers, schemas, configuration, version compatibility, and migration behavior.
- **Security and privacy:** trust boundaries, authorization, input handling, sensitive data, secrets, and unsafe defaults. For AI-agent work, inspect untrusted instructions and data, tool permissions, delegated work, and side-effect boundaries. Use `security-engineering` or `authentication-engineering` for deeper security criteria.
- **Data and persistence:** integrity, transaction boundaries, concurrency, query behavior, and migration safety. Use `database-engineering` for detailed database review.
- **Tests:** coverage of changed behavior, meaningful assertions, isolation, and whether test claims match what ran. Use `testing-engineering` for test design.
- **Maintainability:** complexity, duplicated logic, unclear ownership, and consistency with local architecture. For frontend/mobile work, include user-visible states and platform-specific behavior; for CLI work, include argument, exit-code, and configuration behavior. Use `backend-architecture` or `language-engineering` when their domain is material.
- **Operations and reliability:** failure behavior, observability, resource limits, deployment and recovery impact. Use `devops-infrastructure` for detailed operational review.

## Evidence standard

For a **confirmed defect**, point to reproducible behavior, a concrete code path, a failing check, or another direct observation that violates an approved requirement or causes harm. Explain how the evidence leads to the impact.

For a **plausible risk**, state the condition that would make it occur and what evidence is missing. Do not phrase it as a confirmed defect.

For an **optional improvement**, explain its value and keep it separate from defects. Do not use severity labels to make a preference sound like a requirement.

Check findings against nearby code and relevant Git changes. Record uncertainty where the repository or environment does not let the behavior be verified. Never expose secret values; describe the location or type of exposed material without reproducing it.

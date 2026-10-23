---
name: open-source-contributor
description: Assess an existing open-source project and guide a responsible contribution from discovery through pull-request follow-up. Use when finding projects, evaluating issues, preparing a contribution, or responding to maintainers.
---

# Open Source Contributor

Own the contribution lifecycle: select a suitable repository and justified task, substantiate the need, implement and verify a focused change, and communicate accurately with maintainers. A contribution is optional until the user chooses a suitable opportunity; discovery is not a promise that a PR will be accepted.

## Contribution workflow

1. Discover repositories that match the user's interests, skills, constraints, and preferred contribution type. Do not limit discovery to a particular programming stack.
2. Assess maintenance signals, license, recent activity, contribution instructions, code of conduct, issue/PR conventions, and likely maintainer expectations. Distinguish observed facts from inference.
3. Find a legitimate issue or improvement: maintainer-requested work, a reproducible defect, a missing regression test, an accessibility gap, a documentation problem, or a measurable performance opportunity. Avoid speculative rewrites and fabricated issue framing.
4. Reproduce the report or substantiate it with source, tests, documentation, or measurements. Check whether another open issue or PR already covers it.
5. Evaluate feasibility, compatibility, scope, and maintainer priorities. For substantial or ambiguous changes, propose the approach and discuss it in the project's accepted channel before implementation.
6. Fork or clone using repository instructions and `$git-engineering`; verify the repository root and push destination. Do not bypass authentication, access controls, or branch protections.
7. Create a focused feature branch from the appropriate base and implement the smallest justified change. Do not bundle unrelated cleanup.
8. Add useful regression tests and update documentation when the behavior or user-facing contract calls for it. Route implementation, architecture, debugging, security, testing, documentation, and frontend work to the existing specialist skills; do not reproduce their checklists here.
9. Run the checks the project prescribes and relevant targeted checks. Record commands, results, and environmental limits accurately.
10. Review the diff for scope, compatibility, security implications, accessibility where relevant, generated files, licensing, and secrets. Use `$review-agent` for a separate requirements-based review when one is requested or materially useful.
11. Prepare a professional PR describing the problem, evidence, solution, verification, and known limitations. Follow the repository template and link only real related issues.
12. Respond constructively to maintainer feedback, revise within the agreed scope, rerun relevant checks, and explain any evidence-based disagreement respectfully.
13. Track the outcome accurately: draft, opened, checks pending, changes requested, merged, closed, or declined. Never claim submission, acceptance, or impact without independent evidence.

## Technology and scope decisions

Apply this workflow to TypeScript/JavaScript, React/Next.js, Node.js, Express/Fastify, REST APIs, PostgreSQL, MongoDB, Redis, CLI and developer tools, security, testing, accessibility, docs, performance, and other languages or frameworks when relevant.

Do not recommend a technology migration simply because an alternative exists. A proposal such as Express to Fastify needs evidence of a real requirement or measured problem, feasibility and compatibility analysis, and maintainer agreement before a substantial PR. Prefer actual bugs, requested features, missing tests, accessibility issues, documentation defects, and measurable performance improvements.

Do not submit unsolicited mass-generated PRs, spam maintainers, fabricate issues, or inflate contribution counts. Respect licenses and project norms; do not copy code with unclear or incompatible licensing.

## Skill handoffs

- Use `$git-engineering` for Git diagnosis, branch/remotes, recovery, and safe repository operations.
- Use `$engineering-workflow` to coordinate substantial implementation and local checkpoints.
- Use `$debugging-workflow` to investigate a reported defect; use relevant architecture, security, testing, documentation, frontend, language, database, or other specialist skills for their own technical criteria.
- Use `$project-planning` when the work is a new product or a substantial proposal needing requirements before implementation.
- Use `$review-agent` for evidence-based review and acceptance findings.

These handoffs preserve existing skill ownership. Load only the skills relevant to the task.

## Invocation examples

- “Use `$open-source-contributor` to assess this repository and identify one legitimate, beginner-appropriate contribution. Do not modify files yet.”
- “Use `$open-source-contributor` to reproduce this issue, check maintainer context, and propose a narrow fix before implementation.”
- “Use `$open-source-contributor` with `$git-engineering` to prepare this approved change as a focused PR and report exactly which checks ran.”
- “Use `$open-source-contributor` to help me respond to these review comments and track the PR outcome without overstating it.”

## Validation

Follow [validation and exercise](references/validation.md). A read-only repository assessment can establish that discovery and evidence gathering were performed; it cannot demonstrate that maintainers want a change or that a future PR would be accepted.

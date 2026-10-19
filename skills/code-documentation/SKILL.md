---
name: code-documentation
description: Document and explain code across languages using concise, implementation-grounded comments, API docs, and repository guidance. Use when adding, updating, reviewing, or explaining source code documentation.
---

# Code Documentation

Document code so users and maintainers can understand its contracts, constraints, and intent without changing its behavior. Use this skill for source comments, docstrings, API documentation, and focused code documentation reviews. For a plain-language explanation of technical concepts or a system, use `$plain-english-explainer`; this skill is for documentation grounded in a particular codebase.

## Choose a mode

- **Review mode** (when asked to review comments or code docs): Find missing, outdated, misleading, and redundant comments. Report each finding with its location, evidence, impact, and recommendation; distinguish confirmed inaccuracies from optional improvements. This mode is read-only. Use `$review-agent` for broader reviews against approved requirements.
- **Document mode** (when asked to add or update documentation): Add concise comments, docstrings, API docs, or repository guidance only where they materially help the reader. Explain non-obvious logic, assumptions, constraints, algorithms, and side effects. Follow [language documentation standards](references/language-standards.md), keep public contracts accurate, and preserve executable behavior.
- **Explain mode** (when asked how selected code works): Explain it in plain English, tracing control flow, data flow, dependencies, and design decisions. Give examples when they help. Do not modify files unless explicitly asked.

For general technical teaching, use `$plain-english-explainer`. For implementation, testing, security, or other engineering concerns beyond documentation, use the relevant engineering skill; do not reproduce its workflow here.

Do not reproduce credentials, secret values, or private data in documentation, findings, or explanations.

## Document and review workflow

1. Inspect the relevant implementation, exports, types, tests, and nearby documentation. Identify project conventions and the intended readers before choosing where documentation belongs.
2. Verify behavior and public contracts from code and available evidence. Distinguish confirmed facts from assumptions; ask or qualify when a contract cannot be established.
3. Document only what helps readers: public behavior, inputs and outputs, side effects, errors, preconditions, important constraints, non-obvious algorithms, assumptions, and design reasons. Prefer self-documenting code over comments that merely narrate statements.
4. Follow the language and repository conventions. Consult the ecosystem's current guidance if the project uses another language or a specialized tool.
5. Keep the documentation and implementation consistent. Do not change executable behavior just to add documentation. Do not include credentials, secrets, or private data.
6. Review the result with the [documentation review checklist](references/review-checklist.md). Validate links and rendered/generated documentation when the change or project supports it. Follow [validation instructions](references/validation.md) and report checks actually run.

## Documentation choices

- Use inline comments for non-obvious rationale, invariants, algorithm choices, constraints, or workarounds. Explain why a surprising choice is necessary and, when useful, what would break if it changed.
- Use docstrings and API comments for reader-facing contracts. Keep parameter names, return behavior, exceptions, side effects, defaults, and limits aligned with the implementation.
- Use repository documentation for setup, usage, architecture, and workflows that span multiple files. Link to the authoritative explanation instead of duplicating it.
- Treat the implementation as the source of truth; update affected docs with code changes and remove or revise explanations when behavior changes.
- Keep comments concise and technically accurate. Avoid blanket rules to document every function or line, and avoid promising guarantees the implementation does not provide.

## Explain accurately

In Explain mode, define unfamiliar terms briefly when needed. Separate observed behavior from inference, and do not claim behavior that was not checked.

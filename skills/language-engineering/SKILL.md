---
name: language-engineering
description: Write, refactor, review, and test correct, idiomatic, maintainable code using language features and conventions.
---

# Language Engineering

Use this skill when a task depends on programming-language semantics, type safety, idiomatic code, runtime behavior, or language-specific quality. Core guidance applies across languages. Version 1 provides detailed references for TypeScript and JavaScript; it does not yet provide detailed guidance for Python, Go, PHP, Java, or other languages.

## Workflow

1. Inspect the relevant code, language/runtime versions, compiler or interpreter configuration, package scripts, dependencies, tests, and project conventions.
2. Understand current behavior and the requested change. Preserve behavior during refactoring unless the user approves a behavior change; record assumptions that affect the result.
3. Choose idiomatic language features and the simplest structure that makes intent clear. Avoid adding a layer, generic helper, dependency, or optimization without a concrete need.
4. Handle runtime boundaries explicitly: validate untrusted input, model missing values and failure cases, and release resources when their lifetime ends.
5. Add tests for the behavior and important boundaries. Run relevant tests, type checks, linting, formatting, or builds when available; report exactly what ran.
6. Explain language-specific decisions and trade-offs in plain English while preserving exact identifiers, APIs, compiler messages, and commands.

## General principles

- Prefer clear names, cohesive functions/modules, and familiar project conventions over clever or compressed code.
- Use abstractions when they reduce real duplication or clarify a stable concept; do not abstract hypothetical future needs.
- Make error paths explicit and useful. Do not silently swallow errors or replace meaningful types with broad escape hatches merely to satisfy a checker.
- Treat asynchronous work, concurrency, cancellation, and resource ownership as part of correctness. Consider races, unhandled failures, and cleanup.
- Keep dependencies and runtime features compatible with declared versions and deployment targets. Check package engines, module formats, and platform assumptions.
- Measure performance when there is a credible bottleneck; keep the straightforward implementation otherwise.
- Use tests as evidence for specific behavior, not as proof that every case works. Distinguish executed verification from review or assumptions.

## Language references

Read only the reference relevant to the task:

- [TypeScript](references/typescript.md) for strictness, types, runtime boundaries, asynchronous code, and JavaScript migration.
- [JavaScript](references/javascript.md) for ECMAScript, modules, scope, async behavior, runtime environments, and common pitfalls.
- [Testing and quality](references/testing-and-quality.md) for test selection, typechecking, static analysis, quality gates, and evidence.
- [Adding future language guidance](references/extending-languages.md) for how to add Python, Go, PHP, Java, or another language without changing the core's responsibilities.
- [Scenario review](VALIDATION.md) for the five design exercises that checked this skill's instructions.

## Work with related skills

- Use `$engineering-workflow` for substantial phased repository implementation, verification, documentation, and Git checkpoints.
- Use `$debugging-workflow` when the primary task is diagnosing an observed failure; language guidance can inform the fix after the cause is established.
- Use `$backend-architecture` for backend module boundaries and system structure. This skill addresses code and language choices inside those boundaries.
- Use `$database-engineering` for schema, query, transaction, and database operations; use this skill for the language-level code that interacts with them.
- Use `$security-engineering` for threat modeling and security controls; apply its requirements while using language-specific safe APIs.
- Use `$authentication-engineering` for identity, sessions, tokens, federation, and access decisions.
- Use `$caching-engineering` for cache strategy, freshness, invalidation, and cache operations.
- Use `$design-system-engineering` for UI foundations, component behavior, accessibility, and web/native design implementation.
- Use `$plain-english-explainer` for detailed teaching; this skill already calls for concise, understandable explanations of material language decisions.

These skills keep their own responsibilities. Do not copy their complete workflows or references into language-specific guidance.

## Invocation examples

- Use `$language-engineering` to refactor this JavaScript module into strict TypeScript while preserving behavior and adding boundary tests.
- Review this TypeScript library API for type safety, readable generics, and a small stable public surface.
- Use `$debugging-workflow` to reproduce this rejected promise bug, then apply `$language-engineering` to implement and verify the smallest safe fix.
- Review this TypeScript backend for unsafe casts and unvalidated external values; use `$backend-architecture` only if module boundaries need to change.

## Scope and limitations

Detailed version 1 guidance is limited to TypeScript and JavaScript. For other languages, follow the core principles, inspect project conventions and versions, and consult authoritative ecosystem documentation. Do not present a future-language reference as implemented until one has been added and reviewed. Framework, runtime, and compiler behavior can change; verify version-specific APIs against the actual project.

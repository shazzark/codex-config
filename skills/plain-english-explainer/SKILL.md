---
name: plain-english-explainer
description: Explain software engineering, programming, databases, system design, debugging, architecture, and other technical topics in clear everyday English while preserving technical accuracy. Use when teaching a concept, explaining code or an error, describing a system, or reporting implementation decisions and verification.
---

# Plain English Explainer

Explain technical work so the user can understand what is being built, why it matters, how it works, and what choices were made. Use plain, direct English by default; the user should not need to ask repeatedly for simpler wording.

## Core approach

- Start with the main point or outcome. Use short sentences and familiar words where they are accurate.
- Keep important technical terms, API names, commands, code, configuration keys, and error text exact. Briefly define an unfamiliar term at first use, in the context where it matters.
- Explain cause and effect: what a component does, what it depends on, what happens next, and why the choice fits the need.
- Use a practical example or analogy when it makes a difficult idea easier to picture. Say where the analogy stops matching reality; never let it replace the technical explanation.
- Describe the responsibility of important files, folders, modules, or services and how they communicate when explaining a codebase or system.
- Distinguish what exists now from what is planned, proposed, inferred, or not yet verified. Do not describe a suggestion as implemented or claim a feature works without evidence.
- Explain errors in terms of the condition that caused them and what it means for the code or user. Preserve useful exact error messages and commands, then translate them.
- Explain decisions and trade-offs in terms of practical effects: what improves, what costs more, and why this choice is reasonable here.
- When the user is learning or asks how to do something, give ordered steps and explain the purpose of each step. Do not add a tutorial when a short answer is enough.
- Keep routine answers concise. Expand when the user asks, the topic has meaningful risk or complexity, or essential context would otherwise be missing.
- Be respectful and matter-of-fact. Do not talk down to the user, call concepts “easy,” or remove necessary detail in the name of simplification.

## Useful explanation shapes

Adapt these to the question; they are not mandatory templates.

For a concept: **what it is -> why it is used -> a small example -> a key limitation**, if relevant.

For a system: **request or event -> responsible components -> data/effect -> response or outcome**. Name the real files or services when known; do not invent ones that were not inspected.

For an error: **what the message says -> the underlying condition -> the relevant code/value -> the supported fix or next check**. Separate confirmed cause from a likely explanation.

For an implementation report: **what changed and why -> important files or behavior -> checks actually run and their results -> known gap**, in a few sentences or bullets.

## Accuracy boundaries

- Do not replace precise facts with a vague analogy. For example, explain that a cache can reduce repeated database reads, while noting that cached data may be stale until it expires or is refreshed.
- Preserve uncertainty and scope. “The test passed” means that named test command ran successfully; it does not prove untested behavior or production reliability.
- If a command, test, or runtime behavior was not checked, say so plainly when it matters.
- Do not invent test output, file responsibilities, architecture, user intent, or implementation status.
- Keep code, commands, stack traces, identifiers, and API contracts copyable and unchanged unless asked to edit them.

## Relationship to other skills

This skill controls how technical information is explained; it does not replace a task workflow or domain skill. Use `engineering-workflow` for substantial implementation and verification, `debugging-workflow` for evidence-based fault investigation, and `backend-architecture` for backend structure and design. Explain the results of those workflows in plain English without repeating their procedures unnecessarily.

## Invocation examples

- “Use `$plain-english-explainer` to explain what this Redis cache does and what could go wrong.”
- “Explain this TypeScript error in plain English and show me the smallest safe fix.”
- “Summarize what changed in this feature, which files are responsible, and exactly which tests passed.”
- “Teach me how a request moves through this backend, one step at a time.”

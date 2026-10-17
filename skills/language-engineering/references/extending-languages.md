# Extending language guidance

Add detailed language support only when it can provide accurate, useful ecosystem guidance. Keep the existing `SKILL.md` principles language-independent and preserve TypeScript/JavaScript-specific advice in their references.

For a future language such as Python, Go, PHP, Java, or another language:

1. Add one focused file at `references/<language>.md` (use a clear lowercase name).
2. Cover the language/runtime versions and project configuration to inspect; idiomatic names, module/package organization, types or dynamic boundaries, errors, resource ownership, concurrency, dependencies, and version compatibility.
3. Include language-specific failure patterns, safe migration or interoperability notes where useful, and verification commands only when they are not assumed to exist in every project.
4. Link the new reference from `SKILL.md` and add a focused scenario review to `VALIDATION.md` only when it meaningfully exercises the new guidance.
5. Verify every language-specific recommendation against the target project's actual versions and conventions. Prefer current primary documentation for details that change.

Until a reference is added and reviewed, say that the skill provides foundational principles only for that language. Do not imply detailed Python, Go, PHP, Java, or other ecosystem guidance currently exists.

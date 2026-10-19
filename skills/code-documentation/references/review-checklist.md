# Documentation Review Checklist

Use the applicable questions; skip items that do not fit the change.

- Does the documentation help its intended reader understand a contract, decision, constraint, or workflow?
- Are comments missing where they would explain important non-obvious behavior, outdated after code changes, misleading, or redundant with clear code or authoritative documentation?
- Is every factual statement supported by the implementation, tests, or an explicit requirement?
- Are public API descriptions accurate for names, parameters, defaults, results, errors, side effects, and limits?
- Do comments explain non-obvious reasoning or constraints instead of repeating the code?
- Are assumptions labeled, and are implementation guarantees stated only when verified?
- Does the wording match repository and language conventions and any generated-doc tooling?
- Are links valid and examples consistent with current code?
- Does the documentation avoid secrets, private data, redundant copies, and unnecessary per-line commentary?
- Did the change leave executable behavior untouched? If source files changed, inspect the diff for changes outside comments/docstrings and report any uncertainty.

For review findings, identify the affected file or symbol, explain the mismatch or omission, and prioritize reader impact. Separate confirmed inaccuracies from optional improvements.

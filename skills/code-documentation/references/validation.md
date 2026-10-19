# Validation Instructions

Validation should match the changed documentation and the repository's tooling. Do not claim generated docs, links, or examples were checked unless the relevant command or inspection was performed.

## For this skill package

Run the official Codex skill validator in Python UTF-8 mode:

```powershell
python -X utf8 "$env:USERPROFILE\.codex\skills\.system\skill-creator\scripts\quick_validate.py" "skills/code-documentation"
```

Check every relative Markdown link in `SKILL.md` and the references: resolve it relative to the file containing the link and confirm that the target exists. External links should point to authoritative documentation and can be opened when currency or exact behavior matters.

## For code documentation changes

- Inspect the final diff to confirm changes are limited to requested documentation or comment/docstring text. If code changed too, explain why and verify behavior separately.
- Run the project's documentation build, formatter, or link checker only when configured and relevant. Do not install dependencies or add tooling solely to validate a small documentation edit without a clear need.
- For generated API documentation, build the affected target and inspect representative output when practical.
- For explain-only work, trace claims back to the selected code and state any parts that are inferred or unverified.
- Report the exact checks run and their results; distinguish review from automated validation.

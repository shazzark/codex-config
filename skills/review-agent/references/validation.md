# Validation Instructions

Run the official validator shipped with the installed `skill-creator` skill in Python UTF-8 mode:

```powershell
python -X utf8 "$env:USERPROFILE\.codex\skills\.system\skill-creator\scripts\quick_validate.py" ".\skills\review-agent"
```

If the Python launcher is named `py` on Windows, the equivalent is:

```powershell
py -3 -X utf8 "$env:USERPROFILE\.codex\skills\.system\skill-creator\scripts\quick_validate.py" ".\skills\review-agent"
```

From a POSIX shell, use `python3 -X utf8` and the corresponding path under `~/.codex/skills/.system/skill-creator/scripts/quick_validate.py`.

The official validator checks the skill entrypoint's frontmatter, naming, and unfinished scaffold placeholders. It does not check Markdown links, references, quality of review decisions, installation, or source/install parity. Check those separately: resolve every local Markdown link, inspect the README entry and all resources, confirm the intended integration boundaries, and compare SHA-256 hashes after installation. Record each check and its actual result; a manual check must not be reported as an official-validator pass.

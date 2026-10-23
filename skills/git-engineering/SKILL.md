---
name: git-engineering
description: Diagnose Git repository problems, choose safe repairs, recover work, and guide branch and remote collaboration. Use for Git status, history, branch, remote, push, merge, rebase, conflict, authentication, or recovery questions.
---

# Git Engineering

Own Git-specific diagnosis and repository operations. Begin with evidence and prefer read-only inspection. Do not bypass GitHub authentication, repository permissions, branch protections, or project contribution rules.

Select the mode that fits the request:

- **Diagnose:** establish the repository root and current directory, inspect relevant status, branches, remotes, upstreams, and history, then explain the likely cause and confidence in plain English.
- **Repair:** propose the smallest safe change, preserve uncommitted work, commits, unrelated branches, and remote history, then verify the resulting state. Read [repair and recovery](references/repair-and-recovery.md) before recovery, history edits, or other stateful repairs.
- **Collaborate:** guide fork/upstream setup, feature branches, commits, pull requests, review updates, and synchronization. Respect the target repository's contribution instructions; see [collaboration workflow](references/collaboration.md).

For ordinary diagnosis, inspect before mutating. Treat unknown changes as valuable. Never force-push, hard reset, run destructive `git clean`, rewrite published history, or delete a branch without the user's explicit authorization. Explain the specific loss or remote effect before requesting authorization for such an operation; authorization must cover that operation, not merely the general repair task.

Prefer commands that state their target explicitly. In PowerShell, quote paths with spaces and use `Set-Location -LiteralPath` where needed. Translate commands to the user's shell when useful; most Git commands are cross-platform. Read [diagnosis and command safety](references/diagnosis-and-command-safety.md) for command patterns and Windows details.

Use `$debugging-workflow` when the main task is an application or test defect and Git is incidental. Use `$engineering-workflow` for substantial software changes and checkpoints; this skill owns only Git-specific decisions. Use `$open-source-contributor` for the complete contribution lifecycle.

## Invocation examples

- “Use `$git-engineering` in Diagnose mode to explain why this push was rejected; inspect without changing the repository.”
- “Use `$git-engineering` in Repair mode to recover the commit I lost after a rebase. Preserve the current worktree and explain each state change.”
- “Use `$git-engineering` in Collaborate mode to configure my fork and upstream remote, then show the safe branch workflow.”
- “Teach me what these Git commands do before I run them.”

## Validation

Follow [validation and exercise](references/validation.md). A simulated repository can verify Git reasoning and local state handling; it cannot establish remote authentication, GitHub permissions, or service behavior.

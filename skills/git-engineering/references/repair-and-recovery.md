# Repair and Recovery

Read this before changing repository state. Start with a fresh status and preserve a copy or a recoverable reference to user work when an operation may move or replace it.

## Choose the least disruptive repair

1. Confirm the repository root, branch, status, operation in progress, and intended result.
2. Inspect relevant commits and configuration. Use `git diff` and `git diff --cached` before any operation that could alter the index or worktree.
3. Explain the proposed state change and its likely effect. If it is destructive or rewrites published history, stop until explicit authorization for that exact action is given.
4. Perform one bounded repair at a time, checking status and history after each stateful step.
5. Verify the intended branch, upstream, commit graph, worktree, and index. Run project checks only when they are relevant and authorized.

## Common repair patterns

- **Interrupted merge/rebase/cherry-pick:** inspect status to identify the operation. Resolve conflicts deliberately; continue with the matching `--continue` command only after reviewing the staged diff. Use the matching `--abort` only when the user intends to abandon the operation and its consequences are understood. Abort can discard conflict-resolution work made during that operation.
- **Recover a lost commit:** inspect `git reflog --date=iso` and the relevant commit with `git show --stat --oneline <commit>`. Preserve it first using a new recovery branch or a safe reference if that matches the user's goal. Reflogs are local and expire; they are not a substitute for backups. Do not reset a branch to a reflog entry unless the user explicitly authorizes that movement.
- **Move commits between branches:** inspect the source and destination graphs, then use a targeted cherry-pick when preserving the original branch is important. Resolve conflicts and verify the resulting commit. Cherry-pick creates new commit identities; it does not move or delete the originals.
- **Synchronize with upstream:** fetch the intended remote, inspect ahead/behind commits, and follow repository policy for merge or rebase. Prefer preserving history where policy is unclear. Do not use `--force` to make local and remote branches appear aligned.
- **Correct tracking/configuration:** verify the exact remote and branch names, then change only the necessary `remote`, `branch.*`, or `push.default` setting. Use `git config --local` for repository-specific changes when appropriate. Inspect the resulting value and origin with `git config --show-origin`.
- **Detached work:** if commits or edits exist at the detached commit, preserve them on a named branch before changing references. If there are uncommitted files, inspect them and make a suitable backup or commit only if the user wants that action.
- **Nested repository:** determine whether it is a declared submodule, an intentional embedded repository, or accidental nesting. Inspect `.gitmodules` and `git submodule status`; never delete nested `.git` metadata as cleanup.

## Operations requiring explicit authorization

Never run these without explicit authorization, even if a generic request says “fix it”:

- `git push --force` or `git push --force-with-lease` (can replace remote history and affect collaborators).
- `git reset --hard` (moves the current branch and discards tracked worktree/index changes).
- Destructive `git clean` (deletes untracked files and directories, which may contain user work).
- Rewriting published history with rebase, amend, filter, or similar operations (changes commit identities and may disrupt collaborators).
- Deleting a branch that may contain unmerged or otherwise unpreserved work.

Before asking for authorization, identify the exact target, effect, affected work or collaborators, and a safer alternative if one exists. Do not frame authorization as a routine confirmation for harmless inspection.

## Verification record

Report the final branch and upstream, `git status --short --branch`, relevant before/after commit relationship, and checks actually run. State plainly if remote state was not fetched or could not be verified. Never imply a local check proves that a remote push, permission, or CI run succeeded.

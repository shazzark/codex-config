# Git Safety

Read this reference when the task involves modifying a Git repository, creating checkpoints, recovering work, or preparing changes for push/release.

## Protect existing work

Treat unknown modified, staged, and untracked files as potentially valuable user or agent work.

Before repository-level mutations, inspect as appropriate:

- `git status`
- `git diff`
- `git diff --cached`
- recent `git log`

Do not discard changes merely because they are incomplete or were created by another session.

## Destructive operations

Do not casually use operations such as:

- `git reset --hard`
- `git clean -fd`
- restoring or checking out over unknown changes
- deleting branches containing unmerged work
- rebasing or rewriting shared history
- force pushing

Use destructive operations only when they are genuinely required, their consequences are understood, and the user has authorized the destructive effect when authorization is needed.

## Checkpoint commits

A checkpoint commit should represent a coherent and verified unit of work.

Before committing:

1. run the appropriate verification gates,
2. inspect `git status`,
3. inspect the relevant diff,
4. remove accidental/debug/generated changes that should not be committed,
5. synchronize execution documentation when the repository uses it.

Prefer focused commit messages that describe the completed engineering outcome.

Examples:

`feat: add contextual accessibility analysis`

`fix: preserve alpha during color conversion`

`docs: finalize public color contracts`

Avoid vague checkpoint messages such as:

`changes`

`updates`

`work`

## Commit versus push

A commit records a local checkpoint.

A push publishes commits to a remote repository.

Do not treat these as the same operation.

For long-running multi-phase work, local checkpoint commits can be useful before pushing because they create recoverable states.

Do not automatically push every commit unless the user's instructions or approved repository workflow require it.

Before pushing, verify the intended remote and branch when there is meaningful uncertainty.

Never force-push merely to resolve an ordinary synchronization problem.

# Collaboration Workflow

Use this reference when helping someone contribute with branches and remotes. Confirm the target repository before any commit or push.

## Fork and upstream

For a fork workflow, keep the user's fork as the push destination and the source project as a fetch-only upstream unless the user has legitimate write access and requests another setup. Verify both URLs and permissions from repository metadata; do not assume remote names imply roles.

```text
git remote -v
git remote add upstream <source-repository-url>
git fetch upstream
git branch -vv
```

Only add a remote after checking that the name is unused and the URL is the expected repository. If it already exists, inspect it rather than replacing it. Respect GitHub authentication and branch protections.

## Focused branch and review flow

1. Read the repository contribution guide and any issue-specific instructions.
2. Confirm the current root, clean/dirty state, base branch, remotes, and upstream.
3. Create a focused feature branch from the intended base. Avoid committing to `main` or another protected/default branch unless the repository explicitly expects it.
4. Keep commits scoped to the accepted change. Inspect staged content (`git diff --cached`) before committing.
5. Synchronize using the project's documented merge/rebase policy; inspect the commit graph before and after.
6. Draft a pull request that states the problem, the change, verification, and limitations. Link an issue only when it actually exists and is relevant.
7. Apply review feedback on the same contribution branch where possible. Explain disagreements with evidence and keep maintainers' decisions in view.

For the full issue-to-PR lifecycle, use `$open-source-contributor`. This skill supplies Git mechanics and safeguards.

## Explain commands when teaching

Explain what the command changes, its target, and any material side effect before suggesting unfamiliar or stateful commands. Distinguish `fetch` (updates local remote-tracking refs) from `pull` (fetches and then integrates), `commit` (records local history) from `push` (publishes it), and a remote name from the repository URL it refers to. Prefer short examples and ask the learner to inspect status after state changes.

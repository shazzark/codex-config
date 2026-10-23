# Validation and Exercise

## Structural validation

From the repository root, run the bundled skill-creator validator with Python 3.13.3 in UTF-8 mode:

```powershell
$python313 = 'C:/Users/chido/.pyenv/pyenv-win/versions/3.13.3/python.exe'
& $python313 -X utf8 'C:/Users/chido/.codex/skills/.system/skill-creator/scripts/quick_validate.py' 'skills/git-engineering'
```

Also check frontmatter, internal Markdown links, and shell examples after edits. The official validator checks skill structure and placeholders; it does not prove safe Git reasoning.

## Safe repository exercise

Use a newly created temporary directory only. Do not use a real project or remote. Create a local `source` repository with an initial commit and a local `fork` clone. In the working clone, configure the personal fork as `origin` and the source repository as `upstream`. Create a feature branch whose upstream is deliberately unset. Inspect root, status, branch tracking, remotes, and history. The skill should report the missing tracking relationship without assuming it is an error, explain which remote is intended for fetch and push, and recommend a minimal next step only after confirming the user's intent.

For a rejection simulation, create a bare remote and add a remote-only commit after cloning. Attempting an ordinary push should be rejected as non-fast-forward. Diagnose the distinct local and remote commits before proposing merge or rebase. Do not use a force-push to complete the exercise.

Cleanup: inspect the exact temporary path before deleting it; do not run recursive cleanup against an unchecked or user-provided path.

## Honest report template

Record the temporary repo setup, observed Git output, diagnosis, and final status. State that the exercise verifies local Git graph reasoning only. It does not test GitHub authentication, server-side branch rules, network behavior, or permissions.

## Completed exercise record (2026-10-23)

Created temporary local bare remotes `source.git` and `fork.git`, a working repository, and a second clone that advanced the fork's `main` branch. The working clone created `feature/simulated` with a local commit but no upstream configured for that feature branch. An ordinary push of the feature commit to `fork/main` was rejected with `non-fast-forward`/`fetch first`. After fetching, `git log --left-right --oneline --graph HEAD...fork/main` showed one remote-only commit (`remote update`) and one local-only feature commit. `git status --short --branch` showed the feature branch remained clean; no remote history was overwritten and no repair was performed. The temporary directory was verified to be under `$env:TEMP` and removed after the exercise.

Diagnosis: the push target had a commit missing from the local feature branch, so Git refused to move the remote branch in a non-fast-forward way. A safe next step is to confirm the intended target branch and project merge/rebase policy, integrate the remote change only if appropriate, and verify the resulting graph before retrying a normal push. The unset feature-branch upstream is a separate configuration fact, not the cause of the rejection. This exercise tested local Git graph reasoning only; it did not test GitHub authentication, service permissions, network behavior, or branch protection.

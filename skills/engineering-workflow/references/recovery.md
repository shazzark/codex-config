# Recovery Workflow

Read this reference when work resumes after a Codex crash, terminal closure, interrupted session, context loss, failed agent run, or partially completed implementation.

The goal is to recover from the repository's actual state instead of restarting blindly.

## First principle

Assume existing work may be valuable.

Do not reset, clean, overwrite, or recreate files until the current repository state has been inspected.

## Recovery sequence

Start by establishing what survived.

Inspect as appropriate:

1. `git status`
2. `git diff`
3. `git diff --cached`
4. recent `git log --oneline`
5. repository execution documents such as `task.md`
6. relevant changed and untracked files
7. previous verification evidence that is still trustworthy

Determine:

- the last verified checkpoint,
- what was committed,
- what remains uncommitted,
- which phase or objective was active,
- whether the uncommitted work is complete, partial, or broken,
- what verification still needs to run.

## Resume instead of restart

If useful implementation already exists, inspect and continue it.

Do not recreate a feature merely because the previous agent or terminal session disappeared.

Do not assume untracked files are temporary or disposable.

Compare current code with the execution plan and recent commits to determine the correct continuation point.

## Verification after interruption

A previous session saying that something passed is not sufficient when the current state may have changed.

Run the verification relevant to the recovered work before declaring it complete or checkpointing it.

If the last completed phase is already committed and verified, preserve that checkpoint and continue from the next incomplete objective.

If the current phase is partially implemented, finish and verify it before moving forward.

## Documentation recovery

When `task.md` or an equivalent execution tracker exists, compare it with Git history and the working tree.

Do not blindly trust either source alone when they disagree.

Update execution documentation after establishing the real state.

## Recovery outcome

After inspection, explicitly establish:

- last known-good checkpoint,
- current incomplete work,
- next action,
- verification required.

Then continue the normal engineering workflow.

Never use destructive Git cleanup as a shortcut for understanding interrupted work.

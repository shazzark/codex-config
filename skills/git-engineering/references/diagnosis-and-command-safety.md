# Diagnosis and Command Safety

Use this reference for repository inspection and common Git failures. Prefer direct command output over assumptions from a folder name or UI label.

## Establish location and state

From the directory the user identifies, capture:

```text
git rev-parse --show-toplevel
git rev-parse --show-prefix
git status --short --branch
git branch -vv
git remote -v
git branch --show-current
git log -8 --oneline --decorate --graph
```

`git rev-parse --show-toplevel` identifies the repository root; `--show-prefix` shows the path from that root to the current directory. A blank current branch name usually means detached HEAD. Inspect `git status` and recent history before proposing a checkout or branch operation. Use `git status --porcelain=v1` when a compact, script-friendly status is useful.

Then inspect the specific relationship or configuration in question:

```text
git status -sb
git branch -vv
git remote show origin
git config --show-origin --get-regexp "^(branch\.|remote\.|push\.|url\.)"
git merge-base HEAD @{upstream}
git log --left-right --oneline HEAD...@{upstream}
```

An absent upstream is not automatically an error: a local-only branch may be intentional. A remote named `origin` is only a label; verify its URL. Avoid printing environment variables, credential-helper contents, or token-bearing URLs. If a remote URL embeds a credential, do not repeat it; report the exposure and suggest rotating it through the provider's authorized process.

## Diagnose common reports

- **Push rejected as non-fast-forward:** determine whether the remote has commits absent locally and whether the local branch has unique commits. Compare both sides before recommending fetch plus merge/rebase. Check branch protection and repository policy; do not overwrite remote history.
- **Divergent branches:** fetch only when network access is authorized and useful, then inspect the ahead/behind graph. Explain merge versus rebase consequences and follow project policy. A pull strategy setting alone does not resolve the actual commit relationship.
- **Detached HEAD:** identify the checked-out commit and whether it has work worth keeping. Preserve it by creating or moving to a branch only after confirming intent; do not assume checkout can be discarded.
- **Wrong remote or upstream:** compare configured URL, branch tracking, expected repository, and fork/upstream roles. Confirm the intended destination before pushing or changing tracking configuration.
- **Authentication failure:** separate transport reachability from authentication, authorization, credential-helper, SSO, and branch-protection failures using the actual error. Re-authenticate through supported GitHub/SSH/HTTPS mechanisms. Never suggest copying tokens into commands, disabling protections, using another person's credentials, or bypassing access checks.
- **Merge conflict:** inspect `git status` and conflict markers; determine the active merge/rebase/cherry-pick state. Do not resolve by taking all of one side without understanding the intended behavior. Preserve the conflict context and run relevant checks after resolution.
- **Nested repository or submodule confusion:** use `git rev-parse --show-toplevel`, `git status`, and `git submodule status` where relevant. Distinguish an intentional submodule from an accidentally initialized nested `.git` directory. Do not remove nested metadata or convert repository structure without authorization and a backup/recovery plan.

For uncertain diagnosis, state what the evidence shows, what remains unknown, and the next read-only command that would reduce uncertainty.

## PowerShell and cross-platform notes

Git command syntax is generally the same in PowerShell, Command Prompt, Bash, and Zsh. PowerShell examples:

```powershell
Set-Location -LiteralPath 'C:\work\repository with spaces'
git status --short --branch
git -C 'C:\work\repository with spaces' rev-parse --show-toplevel
```

Use single quotes around literal paths; PowerShell single-quoted strings do not expand `$` expressions. Do not use shell redirection or variable interpolation in a way that may expose credentials. Avoid platform-specific path assumptions when a Git command can identify the root.

## Safe information gathering

For a rejected push, collect status, current branch, upstream, remote URL with credentials redacted, local/remote commit relationship, and the exact rejection text. For authentication reports, avoid requesting passwords, access tokens, private keys, or full credential-store output. A user can share the non-secret error text and whether they use SSH or HTTPS.

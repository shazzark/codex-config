# CI/CD with GitHub Actions

## Workflow structure

Inspect existing workflows and required checks before adding or changing CI. Keep jobs focused on the repository's actual package scripts and supported runtimes. A practical pipeline may install from the lockfile, run formatting/linting, typechecking, unit/integration tests, and a production build. Run expensive or privileged jobs only when their value justifies the time and exposure.

Use dependency caching only when it reduces repeated setup and the cache key includes relevant lockfiles/runtime inputs. Treat caches as untrusted performance aids, not as a place for secrets or authoritative artifacts. Upload build outputs as artifacts when a later job/deployment needs the same tested output; record its commit/build identity and avoid rebuilding different bytes for promotion where supply-chain integrity matters.

Pin actions to reviewed versions or immutable commit references according to the repository's security policy; review update ownership and transitive risk. Keep runners and tool versions explicit enough for reproducibility without freezing security updates indefinitely.

## Permissions and secrets

Grant `GITHUB_TOKEN` only the permissions each workflow/job requires; begin read-only and expand narrowly. Keep secrets in protected repository/environment secret storage, expose them only to jobs that need them, and do not print or pass them through untrusted pull-request code. Avoid evaluating untrusted PR title/body/branch text directly in shell source. Do not use privileged events such as `pull_request_target` with untrusted checkout code unless the design is carefully constrained. GitHub documents [secure workflow use](https://docs.github.com/en/actions/reference/security/secure-use) and [token permissions](https://docs.github.com/en/actions/tutorials/authenticate-with-github_token).

Use environment-specific secrets and approvals for protected deployment environments. Check branch restrictions, required reviewers, and plan availability for the repository; do not assume every GitHub feature is enabled on every plan.

## Gates and promotion

Separate validation from deployment. Require relevant quality checks before packaging; require explicit environment protection before production deployment. Promote the same artifact where feasible so tested output is what gets released. Record commit, version, artifact, environment, and deployment result. Make workflow reruns safe, especially when a deployment step may repeat a side effect.

Define rollback/forward-recovery steps before automating deployment. A rollback may be unsafe after a database schema or data change; coordinate with the database owner and use backward-compatible migration sequencing. Never introduce automatic production deploys, publish artifacts, or alter protections without explicit scope/approval.

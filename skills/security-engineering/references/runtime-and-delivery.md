# Runtime, Dependencies, Containers, and Delivery

## Node.js and TypeScript

- Keep Node.js and framework versions supported and patched. Set production mode deliberately and avoid exposing development servers, inspector ports, verbose diagnostics, or source maps publicly without a reviewed need.
- Parse and validate environment configuration at startup. Keep secrets out of source, client bundles, logs, image layers, and build artifacts. In Next.js, only values intentionally public belong in `NEXT_PUBLIC_*` variables.
- Use maintained dependencies, a committed lockfile, and reproducible installation in CI. Review advisories and transitive impact; do not treat a clean `npm audit` as proof of safety or apply breaking upgrades blindly.
- Limit request bodies, upload size, execution time, concurrency, and outbound resource use. Handle rejected promises and errors without leaking internals or leaving requests in an unsafe partial state.
- TypeScript types are erased at runtime. Validate untrusted values at ingress and avoid unsafe casts that bypass the actual check.

## Express, Fastify, and Next.js deployment

- Check middleware/hook order, route coverage, proxy trust, body parsing, cookies, CORS, error handlers, and request timeouts in the deployed framework version.
- Express and Fastify controls must cover every route that exposes the protected capability. Framework-level defaults do not replace endpoint authorization.
- For Next.js, review all server entry points and their caching behavior. Do not rely on Proxy/Middleware redirects as the sole authorization check. Prevent cross-user or cross-tenant leakage through cached pages, fetches, or data-access functions. Keep sensitive modules server-only and do not serialize secrets to client components.
- Terminate TLS only at an approved boundary and protect traffic between proxy and application when the network threat model requires it. Trust forwarded headers only from known proxies.

## Docker and runtime isolation

- Use maintained, explicit base-image versions and rebuild them for security updates. Keep images small enough to reduce unnecessary packages, while preserving needed security tooling and diagnosability.
- Run as a non-root user, drop unnecessary capabilities, avoid privileged mode and host mounts, and use a read-only filesystem where practical. Bound CPU, memory, processes, and writable storage.
- Do not put credentials in Dockerfile `ARG`/`ENV`, build context, image layers, or logs. Use the build platform's secret mechanism and runtime secret injection; rotate any credential exposed in an image or build log.
- Scan images and deployment manifests. Treat scan results as evidence to triage, not a complete runtime security assessment.

## Dependencies and CI/CD

- Review direct and transitive dependencies, maintainer/release health, package lifecycle scripts, lockfile changes, provenance where available, and known advisories. Use supported versions and prioritize reachable, exploitable issues by impact and exposure.
- Give CI jobs only the repository, environment, and token permissions they need. Separate untrusted pull-request jobs from jobs holding release secrets; protect deployment credentials and approval boundaries.
- Pin or constrain third-party CI actions according to repository policy. Treat workflow expressions, artifacts, caches, build outputs, and test data as possible untrusted inputs across privilege boundaries.
- Run appropriate secret, dependency, static-analysis, and container checks. Prevent secrets from being printed in diagnostics and preserve review of security-sensitive changes.
- Deploy with secure environment defaults, restricted network access, controlled administrative access, and a rollback or forward-fix plan. Keep development configuration separate from production.

## References

- [OWASP Node.js Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Nodejs_Security_Cheat_Sheet.html)
- [OWASP Docker Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html)
- [OWASP Node.js Docker Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/NodeJS_Docker_Cheat_Sheet.html)
- [OWASP Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)
- [OWASP CI/CD Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/CI_CD_Security_Cheat_Sheet.html)
- [OWASP Vulnerable Dependency Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Vulnerable_Dependency_Management_Cheat_Sheet.html)

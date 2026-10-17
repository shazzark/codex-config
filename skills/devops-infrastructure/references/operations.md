# Observability and reliability

## Health and signals

Expose health and readiness signals appropriate to the platform. **Liveness** asks whether a process should be restarted; **readiness** asks whether it should receive traffic. Avoid making a liveness check depend on every downstream service if that could create restart storms. Keep readiness checks bounded and ensure they reflect the ability to serve the relevant workload.

Use structured logs with stable fields such as timestamp, severity, service/version, environment, request/correlation ID, and outcome. Do not log passwords, tokens, connection strings, secret environment values, or unnecessary personal data. Metrics help answer how often/how many/how long (for example request rate, error rate, latency, saturation, queue depth, database connections, Redis memory/evictions); traces link spans across service calls. Add only signals that support decisions or investigation.

Alert on actionable symptoms tied to user impact or recovery risk, with an owner and response instructions. Avoid noisy alerts based only on transient resource spikes. Track deployment markers and service versions to correlate changes with failures.

## Timeouts, retries, shutdown, and failure

Set bounded timeouts for network calls and database operations. Retry only errors likely to be temporary, with bounded attempts, backoff/jitter, and an idempotent operation or deduplication strategy. Avoid retry multiplication across application, proxy, and provider layers. Define what the application does when PostgreSQL, MongoDB, Redis, or an external API is unavailable; fail safely and preserve data invariants.

During graceful shutdown, stop taking new work, allow in-flight requests/jobs a bounded drain period, close listeners and connection pools, flush telemetry if safe, then exit. Test behavior against the platform's termination grace period. Do not let cleanup hang forever.

Plan for resource exhaustion: memory/CPU limits, disk growth, file descriptors, connection pools, worker concurrency, queue backlog, and rate of incoming work. Use capacity measurements and growth trends instead of arbitrary scaling targets.

## Backups, incidents, and runbooks

Maintain runbooks for common operational tasks: identify deployment/version, inspect health and logs, check dependencies, pause unsafe work, restore or switch traffic, and contact owners. Keep commands safe and explain whether they are read-only or destructive. Protect runbooks and logs from secrets and production data.

Incident response should record impact, timeline, detection, mitigation, and follow-up actions. A blameless post-incident review focuses on system conditions and concrete improvements. For disaster recovery, document backup scope/frequency/retention, restore authority, isolation, expected data loss/outage, and periodic restore-test evidence. Replicas and backups have different failure modes; do not assume replication replaces backups.

## Troubleshooting service health

First capture the exact symptom, affected deployment and service, timestamps, health state, exit code, and relevant sanitized logs. Follow connectivity from caller to dependency: DNS/service name, network/port, TLS, credentials presence, connection limits, readiness, and application behavior. Check recent changes and resource pressure. Reproduce in local/staging when safe. Change one likely cause at a time and verify the original symptom; do not remove volumes, disable health checks, or restart production systems as exploratory steps without authorization.

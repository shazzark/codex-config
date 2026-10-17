# Networking-engineering scenario review

These six reviews are design exercises to check whether the skill encourages layered diagnosis and safe fixes. No Docker project, live endpoint, DNS zone, or production service was supplied. No network probe, port scan, Docker connectivity test, TLS handshake, or live infrastructure change was performed.

| Scenario | Appropriate diagnostic path and review result | Check type |
|---|---|---|
| Node.js API cannot connect to PostgreSQL in Compose | Check caller context (`localhost` versus Compose service name), shared network, DNS, database listening port/readiness, credentials presence without revealing values, TLS, and pool errors in order. Container-to-container uses service DNS/internal port; host access uses published host port. Do not publish publicly or remove the database volume as a fix. | Design exercise; no Compose stack or database tested. |
| Socket.IO disconnects behind reverse proxy | Check Engine.IO polling/WebSocket transport, upgrade forwarding, proxy idle/read timeouts, heartbeat interval, server close codes, reconnect behavior, and whether multiple instances need affinity or a shared adapter. Don't assume sticky sessions are always required. | Design exercise; no Socket.IO app or proxy inspected. |
| Intermittent backend DNS failures | Capture exact error/time and resolver context from the application container/host; compare DNS results, IPv4/IPv6, TTL/rotation, resolver health, and target reachability. Correlate with service restarts and pool reuse. Do not replace DNS with hard-coded IPs before cause is known. | Design exercise; no DNS query was made. |
| External API timeouts and retry duplicates | Separate DNS/connect/TLS/response timeout. Determine if the remote write may have committed despite lost response. Bound retries/backoff and use API idempotency/deduplication before retrying non-idempotent writes. Avoid retry multiplication. | Design exercise; no external API request was sent. |
| Redis connectivity fails between Docker services | Verify service names, network membership, port/listener, client URI, TLS/auth, readiness, container logs, and resource/connection limits from the caller container. Use authorized local `docker compose` inspection; don't expose Redis publicly. | Design exercise; no Redis service was contacted. |
| Production HTTPS certificate validation fails | Preserve verification. Inspect hostname/SAN, chain, expiry, client trust store/clock, SNI and any TLS-terminating proxy; reproduce only against the authorized endpoint and report sanitized certificate details. Escalate live config changes through the deployment owner. | Design exercise; no production HTTPS endpoint was contacted. |

## Findings and limitations

The scenarios separate DNS, routing, transport, TLS, proxy, and application layers, and the recommended checks are scoped to named authorized systems. They avoid weakening TLS/firewalls or assuming that host connectivity proves container connectivity. These reviews do not confirm any root cause, certificate status, service reachability, Socket.IO behavior, or production fix; actual evidence from the relevant environment is required.

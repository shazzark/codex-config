# Distributed systems, security, and troubleshooting

## Partial failure and resilience

Distributed communication can fail partially: DNS may resolve differently across instances, connections may reset after a write, a dependency may be slow but reachable, or a partition may isolate only some nodes. A successful health check or ping does not prove application protocol success. Give each network operation a deadline; propagate cancellation where safe; limit retries and concurrency; and choose a fallback only when it preserves the product's data and security rules.

Load balancers spread traffic across backends. Reverse proxies route traffic and often terminate TLS. Service discovery maps a stable service name to changing endpoints. Verify the resolver scope, health policy, connection draining, and client connection-pool behavior together. Rate limits and backpressure protect a slow downstream from unbounded work; circuit breakers can temporarily stop repeated failing calls but add state and should be introduced only with a clear recovery policy.

Use structured network logs and metrics without secrets: target service label (not a credential-bearing URL), DNS/connect/TLS/request duration where available, timeout/reset/refusal counts, response status, retry attempt, pool saturation, and trace/correlation ID. Distributed tracing can link a request across services but needs context propagation and sampling. Never log authorization headers, query secrets, full sensitive payloads, or connection strings.

## Security boundaries

Keep TLS certificate-chain and hostname verification enabled. Use `https`/`wss` for sensitive internet communication and authenticated/TLS database connections when required by the network threat model and provider. Restrict listeners, firewall rules, security groups, and container port publishing to required sources and ports. Separate public ingress from private service/data networks.

Treat external URLs, redirects, DNS results, and user-controlled hostnames as untrusted. SSRF defenses need application-level URL/scheme/port validation, destination restrictions, redirect handling, and protections against DNS rebinding; consult `$security-engineering` for the complete policy. Validate WebSocket origin/authentication and message limits; TLS alone does not authorize the user or message.

## Diagnostic commands and scope

Use commands only for a host/service you own or are explicitly authorized to inspect, and prefer passive or single-target checks:

- Windows: `Resolve-DnsName example.internal`, `Test-NetConnection example.internal -Port 443`, `curl.exe -v --connect-timeout 5 https://example.internal/health`.
- Linux/macOS: `getent hosts example.internal` or `nslookup example.internal`; `curl -v --connect-timeout 5 https://example.internal/health`; `nc -vz -w 5 example.internal 443` if installed and authorized.
- Containers: run the equivalent from the actual application container/network namespace; do not assume host DNS, routes, trust store, or proxy settings match.

`ping` uses ICMP and may be blocked even when TCP works, or may succeed while the target application port is closed. `curl -v` can reveal request headers and certificate details; do not share output containing tokens, cookies, private hostnames, or sensitive query data. `nslookup`/`Resolve-DnsName` shows resolver results, not whether the returned endpoint is reachable. `Test-NetConnection` is a narrow TCP reachability check, not an HTTP/TLS/application test. Avoid `nmap`, broad port ranges, packet capture, load tests, or fuzzing unless explicitly authorized and scoped.

## Error clues

- **Name not resolved / NXDOMAIN:** check spelling, resolver, search suffix, split-horizon/private DNS, and whether the caller uses the expected network. Compare address families and resolver context.
- **Connection refused:** the address was reached but no process accepted that port, or a middlebox rejected it. Check listener, port mapping, service name, and target environment.
- **Timeout:** packets or response may be dropped, route/allowlist may block traffic, service may be overloaded, or an operation may hang. Determine whether DNS, connect, TLS, or response timed out before changing retry policy.
- **TLS/certificate error:** inspect hostname/SAN match, chain/intermediate, trust store, expiry/clock, SNI, and proxy interception. Do not disable verification as a fix.
- **CORS error:** browser policy may block JavaScript from reading a response; the request may still reach the server. Compare browser network details with an authorized direct request. Correct server origin/credential policy; do not treat CORS as a firewall or authorization control.
- **HTTP 4xx/5xx:** a server or proxy responded. Identify which hop returned it and inspect safe request IDs/logs before calling it a transport failure.

Reproduce with one authorized destination and known method/path, record timestamps and sanitized output, and compare from caller, container, and host only as needed. On Windows, Linux, Docker Desktop, and Linux Engine, interfaces, resolver files, firewall tools, and `host.docker.internal` behavior differ. Consult the target platform's docs rather than copying host-specific commands blindly.

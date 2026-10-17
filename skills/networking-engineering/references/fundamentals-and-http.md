# Networking fundamentals and HTTP

## A communication path

An IP address identifies a network interface; IPv4 and IPv6 are different address formats and can coexist. A hostname is a human-readable name resolved through DNS to one or more addresses. A port identifies a service endpoint on an address. A socket is an operating-system communication endpoint, commonly identified by protocol, address, and port. TCP provides an ordered, reliable byte stream; UDP sends independent datagrams without TCP's delivery/order guarantee. Neither protocol alone describes the application message format.

`localhost` normally resolves to loopback addresses on the same machine. It means “this machine/network namespace,” not “the Docker host” or “another container.” A process must listen on an interface reachable by its caller. Private/public address, route, NAT, firewall, and interface configuration determine whether packets can travel between networks. Check IPv4 and IPv6 resolution separately when behavior differs.

Latency is the time for data and responses to travel; bandwidth is the available data rate; packet loss means packets fail to arrive. A connection can be slow or intermittent even when DNS and basic reachability work. TCP connection setup, TLS handshake, server processing, queueing, and response transfer contribute different delays.

## HTTP and HTTPS

HTTP defines request/response semantics: method, target, headers, body, status, and response. HTTP/1.1, HTTP/2, and HTTP/3 share core semantics but differ in framing and transport. HTTP/2 multiplexes streams over TCP; HTTP/3 uses QUIC over UDP. Do not infer a failure layer from the HTTP version alone. HTTPS is HTTP carried over TLS, which authenticates the server and protects confidentiality/integrity in transit. Keep certificate and hostname verification enabled.

An HTTP status (such as `401`, `404`, or `503`) means an HTTP-speaking peer returned a response. DNS errors, refused connections, resets, TLS handshake errors, and timeouts may happen before an HTTP status exists. Read the exact client error and response status separately.

Node.js clients and agents can pool/reuse sockets. Reuse reduces repeated connection setup but needs finite socket/connection limits, idle connection handling, and lifecycle cleanup. A pool is not a guarantee that a peer keeps an idle connection open; clients should handle a stale socket and make a safe new request if appropriate. Consult the [Node.js HTTP and TLS/network docs](https://nodejs.org/api/http.html) for the installed runtime's behavior.

## Timeouts, cancellation, and retries

Set explicit, bounded timeouts for connection establishment, TLS/request response, and overall operation where the client API supports those separately. Define cancellation ownership so a request abort can stop unnecessary downstream work. A timeout does not prove the remote operation did not complete; a client may lose the response after the server committed a write.

Retry only likely transient failures and only when repeating the operation is safe. Reads and idempotent operations are generally easier to retry than non-idempotent writes. For writes, use an idempotency key or another deduplication/invariant strategy when supported by the API. Bound attempts, use exponential backoff with jitter, respect server retry hints where appropriate, and avoid multiplying retries across clients, proxies, queues, and services. Fail with a clear error when the budget expires.

## Proxies, gateways, and pooling

An outbound proxy changes the route a client takes; inspect proxy environment/configuration and whether the client honors it. A reverse proxy sits in front of a service and may terminate TLS, route by host/path, buffer bodies, or impose timeouts and size limits. An API gateway may also authenticate, rate-limit, transform, or route requests. Identify which hop generated a response before attributing it to the application.

Forwarded headers such as `Forwarded` and `X-Forwarded-*` are claims supplied by a proxy. Trust them only when requests can arrive through a known proxy boundary; otherwise clients can spoof original host, scheme, or IP. See `$security-engineering` for detailed proxy and SSRF controls.

## Diagnostic order

For a named and authorized target, check in this order and stop at the first failing layer:

1. Is the target name/configuration correct in this caller's environment?
2. Does DNS resolution return expected address families/records?
3. Can the caller route to the address and reach the intended port?
4. Is the service listening and ready on that interface/port?
5. Does the TLS handshake validate hostname, chain, and trust store?
6. Does the protocol request reach the expected proxy/application, and what response/status does it return?
7. Is the failure intermittent under load, connection reuse, or a particular address family?

This sequence narrows cause; it does not justify changing firewall, TLS, or public exposure settings without evidence and authorization.

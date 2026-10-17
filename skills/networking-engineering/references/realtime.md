# Real-time communication

## Choose the transport for the interaction

- **WebSocket** provides a persistent, bidirectional connection after an HTTP upgrade. Use it when both sides need to send messages over one live connection.
- **Server-Sent Events (SSE)** provides a long-lived, one-way stream from server to browser over HTTP and has built-in browser reconnection behavior. Use it when clients mainly receive updates and can send commands through ordinary HTTP.
- **Long polling** repeatedly holds an HTTP request until data or a timeout is available, then opens another request. It can work where persistent connections are unavailable, but adds request overhead and server state.
- **Socket.IO** is a higher-level event library that can use WebSocket and fall back to HTTP long-polling, with its own handshake, heartbeat, reconnection, and adapter behavior. It is not wire-compatible with a plain WebSocket client.

Choose based on directionality, browser/client support, proxy behavior, expected connection count, message guarantees, authentication, and operating model. Do not introduce a broker or distributed adapter without requirements such as multi-instance fan-out or cross-process coordination.

## Connection lifecycle and delivery

Treat open, reconnect, heartbeat, close, and server shutdown as explicit states. Keepalive/heartbeat messages help detect dead connections hidden by NATs, proxies, mobile networks, or idle timeouts; set intervals below the smallest relevant idle timeout, with reasonable tolerance. Reconnect with bounded exponential backoff and jitter, stop reconnecting when the client is intentionally closed, and refresh credentials safely.

A successful socket send often means the local stack accepted bytes, not that the remote application processed or persisted a message. If delivery, ordering, or replay matters, define message IDs, acknowledgements, deduplication, sequence/cursor, retention, and resumption semantics. Make handlers idempotent where messages may be retried or delivered more than once.

## Scaling and proxies

Long-lived connections consume file descriptors, memory, and proxy/load-balancer slots. Plan connection limits, per-client message limits, idle handling, backpressure, and graceful draining. Stop reading or queueing without bounds when consumers are slower than producers; apply bounded buffers, drop/coalesce policies, or explicit flow control according to the product contract.

For WebSocket/Socket.IO through a reverse proxy, verify that the proxy forwards the upgrade handshake, supports the required HTTP version, and has suitable read/idle timeouts. Sticky sessions may be needed for Socket.IO's polling transport or in-memory session state when requests can land on different instances. WebSocket-only mode or a shared adapter changes that requirement; inspect the selected version and topology before prescribing affinity. The application still needs a strategy to distribute events across instances if events originate elsewhere.

SSE may be buffered by proxies unless streaming is configured; test actual event timing through the intended path. Verify heartbeat comments/frames, connection timeout, reconnect behavior, and shutdown cleanup through every proxy/CDN hop. Close sessions during logout, credential revocation, and deployment when required by the app's security contract.

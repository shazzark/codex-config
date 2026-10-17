# JavaScript

## Language and module compatibility

Inspect the supported runtime/browser versions, package manager, `package.json` `engines`, build/transpile pipeline, and module format before using newer syntax or APIs. ECMAScript language features and host APIs (Node.js, browsers, workers) are separate compatibility questions. Check both the syntax target and whether the runtime provides required APIs.

Use ESM or CommonJS consistently with the package and consumers. For libraries, inspect `exports`, `main`, `module`, `type`, and declaration/build output conventions before changing public entry points. Avoid accidental mixed-module behavior and test the package in the formats it claims to support where practical.

## Scope, closures, objects, and arrays

Prefer `const` for bindings that are not reassigned and `let` when reassignment is needed; avoid `var` in new code because its function scope and hoisting can surprise readers. A closure is a function together with access to values from the scope where it was created. Closures are useful for callbacks and encapsulation, but long-lived callbacks can also retain large objects or stale state; understand ownership and lifecycle.

Use array/object methods when they make transformation clear. Be explicit about mutation: methods such as `sort` mutate an array, while methods such as `map` return a new array. Avoid clever chaining when intermediate names would clarify a multi-step transformation. Check that external values have the expected shape before destructuring or invoking methods.

## Asynchronous behavior and errors

Promises represent work that will complete later. `async` functions return promises; `await` pauses that function until a promise settles, not the entire event loop. Use `try/catch/finally` around awaited operations whose errors or cleanup matter. Return promises to callers when they own the response; do not start asynchronous work without an intentional error and lifecycle owner.

The event loop runs JavaScript callbacks as work becomes ready. Long CPU-bound synchronous work can block other requests or UI events even when the program uses promises. Promises do not make CPU work parallel. Use worker threads, Web Workers, or platform-specific mechanisms only when workload and measurements justify them.

Use concurrency deliberately. `Promise.all` starts/awaits independent work together but rejects when one promise rejects; other work may still be running. Use `Promise.allSettled` when each outcome must be collected, sequential awaits when order matters, and bounded concurrency for large workloads. Add cancellation/timeouts only with a clear owner and cleanup behavior.

## Runtime validation and resource management

JavaScript has dynamic runtime values. Validate parsed JSON, environment values, user input, database results, and external responses at boundaries. Do not treat a comment, JSDoc annotation, or expected shape as proof of runtime validity.

Release resources according to the API lifecycle: close streams, files, sockets, database clients, timers, and event listeners when no longer needed. Prefer `try/finally`, `using`/`await using` where supported and appropriate, or library-provided scoped cleanup. Check exact runtime support before adopting newer resource-management syntax.

Garbage collection handles many memory allocations but does not close external resources and cannot reclaim objects still reachable from references, timers, or listeners. Avoid unbounded caches, retained listeners, and storing more data than needed. Measure memory behavior before optimizing.

## Node.js and browser differences

Node.js offers server/runtime APIs such as filesystem, process, streams, and server networking; browsers have DOM, window, and web platform APIs with security and lifecycle rules. Code intended for both environments must not assume either global set exists. Keep environment-specific modules or conditional paths explicit and test the supported targets.

## Common pitfalls

- `==` performs coercive comparison; use strict equality unless coercion is deliberately required.
- `||` replaces all falsy values; use `??` when only `null`/`undefined` should trigger a default.
- `forEach(async ...)` does not wait for callbacks; use a loop or aggregate their promises intentionally.
- Array/object spread is shallow, not a deep clone.
- `typeof null` is `"object"`; use a deliberate null check.
- Unhandled rejected promises and callback errors can escape the intended request/error path.
- `JSON.parse` can throw and parses syntax only; it does not validate an application's expected schema.
- Avoid relying on object key order or implicit coercion for business rules unless the specification guarantees the behavior.

# TypeScript

## Configuration and annotations

Inspect `tsconfig.json`, compiler version, build tool, and package scripts before changing configuration. Prefer strict checking for new or migrated code when compatible with the project. `strict` enables a family of checks; understand existing exclusions and incremental migration constraints instead of flipping options blindly. In particular, check how `strictNullChecks`, `noImplicitAny`, module resolution, target, and library declarations affect the codebase.

Let the compiler infer obvious local types. Add annotations at public APIs, exported functions, mutable state with unclear inference, and boundaries where they improve readability or catch drift. Avoid repeating a type the compiler can infer clearly, and avoid type assertions that merely suppress a useful error.

## Modeling values

- Use `interface` or `type` according to project conventions. Both can describe object shapes; `interface` supports declaration merging and extension patterns, while `type` can express unions, intersections, mapped types, and aliases. Choose based on the shape and API, not a universal rule.
- Use generics when a function or type preserves a relationship between input and output types. Keep constraints and inference understandable; avoid generic parameters that do not add a useful relationship.
- Use built-in utility types such as `Pick`, `Omit`, `Partial`, `Record`, and `Readonly` when they express a real transformation clearly. Do not create deep conditional types for simple data shapes.
- Prefer discriminated unions for a finite set of states or outcomes. A shared literal field (for example, `kind`) lets TypeScript narrow each case and can make impossible combinations unrepresentable.
- Use `unknown` for values whose type is not yet known, then narrow or validate them. `any` disables meaningful checking and should be isolated and justified when an interop boundary makes it unavoidable.
- Treat optional properties, `null`, and `undefined` as distinct where the application distinguishes “not supplied,” “explicitly empty,” and “unknown.” Use narrowing, defaults, or explicit result types rather than non-null assertions as a shortcut.

## Runtime boundaries

TypeScript types are removed at runtime. They do not validate HTTP bodies, JSON, environment variables, database rows, queue messages, or third-party responses. Parse external data from `unknown` with explicit checks or a maintained schema-validation library already suited to the project. Keep the validated type connected to the actual parsed value; a cast is not validation.

Define public API inputs and outputs deliberately. Reuse generated or shared contracts when the project has them, and do not assume compile-time types alone keep independently deployed clients and servers in sync.

## Errors and async work

`try/catch` can receive any thrown value; narrow unknown errors before reading properties. Preserve useful cause/context when rethrowing. Use typed result unions when callers need to handle a known set of expected outcomes without exceptions; use exceptions for exceptional failures according to project conventions. Do not create a custom error hierarchy unless distinct handling or metadata needs it.

Use `async`/`await` to make promise flow readable. Await promises whose failures matter, return them when the caller owns handling, and avoid floating promises. Handle cleanup with `finally` or scoped APIs. Use `Promise.all` only when work can safely run concurrently; use sequential or bounded concurrency when order, resource limits, or partial failure matters.

## Modules and common mistakes

Follow the project's module format and import conventions (`ESM` or CommonJS); inspect `package.json`, compiler output, and runtime support before changing exports. Keep imports explicit and avoid circular dependencies or broad barrel exports that obscure boundaries.

Watch for:

- assuming an interface or type alias checks data at runtime;
- widening literals or losing useful inference through premature annotations;
- using `as`, `!`, or `any` to silence errors instead of proving a condition;
- confusing `T | undefined`, optional properties, and explicit `null`;
- mutating values typed as readonly through aliases or unsafe casts;
- forgetting that `Array.prototype.map(async ...)` returns an array of promises;
- catching an error and continuing with an invalid fallback;
- relying on TypeScript's structural compatibility where a runtime brand or validation boundary is required.

## JavaScript-to-TypeScript migration

Migrate in small boundaries. Inspect existing behavior and tests, enable strictness in a manageable path, add explicit runtime checks at external inputs, and avoid converting unrelated modules. Prefer types that describe the real runtime contract. Preserve module behavior and package exports, and run typecheck plus existing tests after each meaningful slice. Track any temporary `any`, suppression comment, or untyped boundary so it can be reviewed later; do not claim a module is fully type-safe while such gaps remain.

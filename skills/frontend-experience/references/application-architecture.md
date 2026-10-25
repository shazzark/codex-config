# Frontend Application Architecture

Choose a structure that fits the product, team, framework, and current change. These are options rather than a required tree. Inspect the repository, route model, dependencies, tests, and existing conventions first. Create only directories and boundaries that solve a current need.

## Choose a project shape

### Small frontend website

For a small brochure site, landing page, or early prototype, keep routing, a few composed components, styles, and assets direct. A small React/Vite project may need only `src/App`, a handful of components, and a stylesheet; add a router, feature folders, state library, or shared component package only when real navigation, reuse, state, or deployment needs justify them. Static pages may not need React at all.

### Growing React or Next.js application

For a product with multiple capabilities, organize feature-specific screens, components, hooks, validation, and API calls together. Keep genuinely reusable UI and infrastructure in a small shared area. In Next.js App Router projects, use `app/` for route composition and route-segment behavior, and keep substantial capability code in `features/` when that makes it easier to reuse or maintain. A feature might contain only the parts it actually needs:

```text
src/
  app/
    orders/[id]/page.tsx
  features/
    orders/
      components/
      hooks/
      services/
      schemas/
      types.ts
  components/
    ui/
    shared/
```

In a Vite app, use the router already chosen by the project and place route definitions/pages according to its conventions; do not create a Next.js `app/` tree. Feature boundaries can still group capability code without a framework-specific router.

### Large frontend platform

For many independently owned domains, applications, or teams, use explicit domain boundaries and shared packages only when ownership, reuse, and release needs are real. A workspace may hold several apps and focused packages such as a UI system, API contract, or stable utilities. Give each package an owner and public entrypoints; avoid a catch-all shared package and avoid extracting code merely to make the tree look modular.

Choose state management by state lifetime and ownership. Keep server data, URL/navigation state, form state, and transient UI state distinct. Add a shared state library only when coordinated client-owned state crosses features enough to justify its rules. Organize tests around feature behavior and real integration boundaries. More packages and boundaries also add dependency management, build, versioning, integration-test, and ownership costs.

## Directory responsibilities

Directory names are conventions, not requirements. Keep feature-specific concerns with their feature unless the project has a clear, established reason to centralize them.

| Directory | Responsibility and when to use it |
| --- | --- |
| `app` / routes | Next.js App Router route segments, layouts, loading/error/not-found boundaries, and composition. In ordinary React, use this only if it is the existing convention; keep route setup with the chosen router. |
| `pages` | Next.js Pages Router routes in an existing Pages Router app, or route-level page components when that is the project's naming convention. Do not add beside `app/` without an intentional migration or mixed-router need. |
| `components/ui` | Reusable visual primitives and stable variants, usually backed by the design system. Keep it small and do not place product workflows here. |
| `components/shared` | Composed components reused across multiple features and not owned by one feature. Keep one-off or feature-specific components local. |
| `features` | Business capabilities such as `accounts`, `catalog`, or `orders`, with their own UI, hooks, schemas, client services, and tests as needed. Omit the wrapper for a small site without meaningful feature boundaries. |
| `hooks` | Truly reusable React hooks that coordinate component state or lifecycle. Feature-specific hooks belong under that feature. Hooks are React functions and must follow React's rules; they are not backend hooks. |
| `context` | Client-side values needed by a subtree when props or a focused state store are less clear. Keep providers scoped, values stable, and updates modest; do not use Context as a default for all state or server data. |
| `providers` | Composition of required app-level providers such as theme, auth display state, or query client. Keep provider setup minimal and wrap only the route/app subtree that needs it. |
| `services` | Frontend API clients and feature operations that adapt backend contracts for UI use. Feature calls usually belong in feature-local `services`; keep shared transport/config separate. |
| `lib` | Shared framework or infrastructure integrations such as an API transport, telemetry setup, or server-only data access. Avoid turning it into a second `utils` or feature bucket. |
| `utils` | Small, pure helpers with proven reuse across features. Keep feature-specific formatting or transformations near their owner. |
| `schemas` / validation | Runtime schemas and UI-facing validation. Keep form rules feature-local; share a schema with the backend only when a real contract/build boundary supports it. Client validation improves feedback but does not replace server validation. |
| `types` | Cross-feature types that lack a natural owner. Prefer types beside their feature or service; types do not validate runtime data. |
| `styles` | Global resets, tokens, typography setup, and truly global styles. Use CSS modules or the existing scoped styling approach for component/feature styles; avoid a global stylesheet of unrelated feature rules. |
| `assets` | Source assets imported or transformed by the build, organized with their feature when specific. Use the framework's static/public directory for files served by stable public URL as appropriate. Preserve licenses and existing asset conventions. |
| `tests` | Cross-feature integration, end-to-end, accessibility, or visual tests when they fit the repository's discovery convention. Co-locate focused component/feature tests when useful; do not duplicate suites. |

Do not combine rendering, API transport, business-specific client orchestration, and global state in one generic module when they have different owners or change for different reasons. Conversely, do not create empty folders or wrappers for layers a project does not need.

## React component and state boundaries

- Compose UI from focused components with clear props and semantic HTML. Extract a shared component for real reuse, an accessibility-sensitive interaction, or a coherent composition—not merely to shorten a file. Prefer composition and named variants over broad boolean-prop combinations.
- Keep transient state near the component that owns it. Lift state to the nearest common owner when siblings must coordinate. Use the URL for navigable/shareable state such as filters or selected records where appropriate.
- Use Context for stable values shared across a subtree, such as theme or current display preferences. Avoid duplicating values that can be derived and avoid putting frequently changing, unrelated state in one broad provider.
- Treat remote/server data separately from local UI state. Use ordinary framework/server fetching where it fits. A client cache such as TanStack Query is useful when the client needs shared server-state caching, background refetch, invalidation, mutation coordination, or optimistic updates; it is not mandatory for every fetch and does not replace a backend API or authorization.
- Put feature API functions near the feature. Centralize only cross-cutting transport behavior such as base URL, headers, safe error conversion, and tracing. Avoid fetching the same data independently in multiple components or making components know backend URL and error details.

See React's [state management guidance](https://react.dev/learn/managing-state) and [TanStack Query overview](https://tanstack.com/query/latest/docs/framework/react/overview) for current library-specific details.

## Next.js App Router

Apply these conventions only to projects using the App Router; do not impose them on React/Vite or an existing Pages Router app.

- `app/` maps route segments and owns layouts and route-level composition. Use `page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`, and `not-found.tsx` when their route behavior is needed. Keep route components focused and move reusable feature behavior to its feature. A route handler is for an HTTP endpoint/BFF need, not a required proxy for every backend call.
- Pages and layouts are Server Components by default. Prefer server rendering and server-side data access when browser interaction is unnecessary. Add a small Client Component boundary for event handlers, browser APIs, effects, or client hooks; keep the boundary low in the tree and pass serializable data across it.
- Keep secrets and privileged data access in server-only modules. Authenticate and authorize every protected server-side read or mutation; hiding a button or trusting client-supplied identity is not authorization.
- Fetch on the server near the route/component that needs data when suitable. Fetch independent resources in parallel and use streaming/loading boundaries where waiting would block useful content. Check the installed Next.js version and its cache defaults before relying on caching or revalidation behavior.
- Use Server Actions/Server Functions for suitable server mutations when they fit the app's architecture, but treat them as callable network entrypoints: validate inputs and enforce authorization inside each action. Use route handlers when an HTTP API contract or external client needs an endpoint. Do not duplicate backend business rules in either adapter.
- Represent expected errors (validation, unavailable record, permission denied) as actionable UI states; reserve error boundaries for unexpected failures. Provide route-appropriate loading, empty, not-found, and error states.

See official [Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components), [data fetching](https://nextjs.org/docs/app/getting-started/fetching-data), [mutations](https://nextjs.org/docs/app/getting-started/mutating-data), and [error handling](https://nextjs.org/docs/app/getting-started/error-handling) guidance for the installed framework version.

## API, forms, authentication, and feedback

- Keep the frontend API boundary distinct from backend business logic. Use typed contracts for developer feedback and validate untrusted runtime responses when their shape or risk warrants it. Coordinate breaking API changes with the backend owner; preserve existing routes and behavior.
- Validate forms for immediate, understandable feedback and validate again on the server. Associate labels and errors with inputs, explain corrections, preserve recoverable input, and expose submitting/success/failure states. Use the project's established form/schema libraries rather than introducing new ones by default.
- Authentication-aware UI controls navigation and presentation. The server/API remains authoritative for authentication and authorization, including resource ownership; client roles or hidden controls must not grant access. Coordinate identity flow details with `$authentication-engineering` and security-sensitive display/data exposure with `$security-engineering`.
- Design loading, empty, error, success, disabled, and pending states as part of each affected flow. Make asynchronous status perceivable and give users a useful retry or next action where possible.

## Visual system, accessibility, and responsiveness

Use the existing semantic design tokens, typography, themes, component variants, and responsive rules. Reuse UI primitives when their behavior fits; do not make application architecture dictate a visual style. `$frontend-experience` owns visual direction and the [25-style library](website-design-styles.md); `$design-system-engineering` owns reusable visual foundations, tokens, component APIs/variants, accessibility, and cross-platform design-system maintenance. Use both where a task crosses those concerns.

Use semantic HTML, keyboard-operable controls, visible focus, meaningful labels, and accessible status/error announcements. Check responsive content reflow, zoom/text sizing, long content, touch targets, and supported viewports. Detailed visual and accessibility guidance belongs to `$design-system-engineering`; the application structure should make those shared components and styles straightforward to consume.

## Performance and verification

- Avoid unnecessary client JavaScript, broad client boundaries, duplicated requests, and premature memoization. Lazy-load expensive or rarely used code when it improves the actual experience. Optimize images/fonts and rendering based on project needs and measured evidence.
- Keep package boundaries and state libraries to the minimum that supports current ownership and scale. For larger platforms, define package public APIs and test consumers at integration boundaries.
- Inspect existing tests and scripts before adding test structure. Test meaningful component behavior, feature flows, API contracts, and route boundaries at useful levels; add browser/e2e or visual checks when they verify real user journeys or visual requirements.
- For a substantial architecture change, run relevant checks and review routing, API contracts, authentication, state ownership, accessibility, and compatibility. Report what was actually verified; a conceptual exercise or static review is not an executable test.

## Architecture selection and existing projects

1. Inspect the current folders, framework/router and versions, dependencies, route and feature boundaries, test setup, and local instructions.
2. Understand project size, team/domain ownership, expected change, rendering/data needs, accessibility requirements, and supported deployment targets.
3. Prefer the established structure if it supports the task. Choose the smallest project shape above that meets requirements and explain meaningful trade-offs.
4. For a new project, recommend only folders and dependencies justified by its requirements. For a mature project, preserve its organization unless restructuring is explicitly requested and justified by evidence.
5. Preserve user modifications, routing, public API contracts, authentication behavior, and other existing behavior. Avoid unrelated moves or renames.
6. Review substantial architecture changes against focused tests and a diff; use `$engineering-workflow` for execution and `$testing-engineering` for test selection.

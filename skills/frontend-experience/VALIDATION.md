# Architecture Scenario Review

These bounded scenarios were reviewed conceptually against the application-architecture reference. They are not executable applications, tests, performance checks, or visual inspections.

## 1. Small React/Vite application

**Scenario:** A small Vite site has a few mostly static pages and one interactive widget.

**Guidance outcome:** Keep the existing router (or simple page composition), a few components, scoped/global styles as needed, and assets. Keep the widget's local state in its component. Do not add feature-package layers, global state, Context, TanStack Query, or a component library without a concrete need. Use the existing tokens and visual style.

**Evaluation:** The small-site option stays direct and avoids Next.js conventions and unnecessary dependencies.

## 2. Growing Next.js App Router application

**Scenario:** A product has catalog, account, and order workflows with server-rendered pages, interactive forms, and authenticated data.

**Guidance outcome:** Let `app/` own segments, layouts, and route loading/error boundaries. Group substantial capability components, hooks, schemas, API calls, and tests under features. Fetch safe page data in Server Components where appropriate; isolate client interactivity to small Client Component boundaries. Validate and authorize each server mutation, represent pending/error/empty states, and use a client query cache only for client-owned server-state behaviors that need it. Reuse design-system primitives and semantic tokens.

**Evaluation:** Framework boundaries support server rendering and feature ownership without duplicating backend policy or imposing client state infrastructure everywhere.

## 3. Mature application with an established structure

**Scenario:** A mature React application uses a stable router, global component conventions, API client, authentication flow, tests, and local modifications; a new settings screen is requested.

**Guidance outcome:** Inspect and extend the existing settings route, API client, authentication checks, component and test conventions. Add feature-local files only where they fit existing ownership. Preserve routes, API contracts, user changes, and established behavior. Do not migrate to Next.js App Router or reorganize the app to match the reference.

**Evaluation:** Existing-project protection takes precedence over preferred templates; structural migration requires explicit request and evidence.

## Scope

All three are conceptual architecture exercises. No executable code, automated tests, browser rendering, accessibility audit, or performance measurement was performed.

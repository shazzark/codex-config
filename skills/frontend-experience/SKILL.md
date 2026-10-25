---
name: frontend-experience
description: Choose visual directions for websites and apply proportionate React/Next.js application architecture guidance while preserving existing organization and behavior.
---

# Frontend Experience

Use this skill when a website needs a clear visual direction, a cohesive page experience, a design-focused audit, or a frontend application structure decision. It guides visual decisions and frontend architecture without replacing product requirements, design-system ownership, or backend architecture.

For React and Next.js project structures, directory responsibilities, data/state boundaries, and architecture selection, read [Frontend application architecture](references/application-architecture.md). Preserve mature project organization unless a structural change is requested and justified.

## Design a new website

1. Inspect the product purpose, audience, content, brand, user journeys, existing implementation, and available assets, requirements, and references.
2. Use [Website design styles](references/website-design-styles.md) to compare only relevant directions. Treat styles, layout patterns, interaction techniques, and surface treatments as composable tools, not a set of mutually exclusive themes.
3. Choose a primary direction and any useful complementary characteristics. Respect a style the user explicitly selected; do not change it for novelty. When no style is selected, ground the choice in the product and audience rather than defaulting to a generic AI-generated landing-page pattern.
4. Explain how the direction informs typography, layout, imagery, color, and interaction. Make trade-offs visible when they affect readability, accessibility, performance, or the user's goals.
5. Implement with the existing design system and project conventions. Verify the rendered result at relevant viewports when browser tools are available; distinguish visual inspection from code checks.

## Audit & Refine an Existing Website

Keep this workflow separate from new-site design. Inspect current pages and behavior before recommending changes. Preserve functionality and brand identity. Evaluate typography, spacing, hierarchy, layout, imagery, responsiveness, accessibility, and consistency, and cite concrete examples from the inspected pages. Recommend the smallest useful refinements first.

Do not modify backend logic, APIs, authentication, database schemas, or unrelated functionality as part of a visual audit. Obtain user approval before a substantial redesign or changes that materially alter the established visual identity. Verify the rendered result when browser tools are available.

## Work with related skills

- Use `$design-system-engineering` for shared tokens, components, accessibility behavior, and system-level design decisions. This skill selects and applies website-level visual direction.
- Use `$backend-architecture` for backend API, persistence, authorization, and service boundaries; keep UI state and rendering in the frontend.
- Use `$engineering-workflow` for substantial repository implementation, proportional planning, verification, and diff review.
- Use `$review-agent` when an evidence-based review against approved requirements is requested or useful.
- Use `$testing-engineering` to select meaningful behavior, responsive, accessibility, or visual checks; do not treat screenshots or automated checks as proof of every user experience.
- Apply `$code-documentation` when implementation comments materially clarify non-obvious behavior.

Read the style library only when choosing or comparing visual directions. For an existing site, begin with its actual pages and identity rather than applying the catalog as a redesign checklist.

Architecture exercises and their verification scope are recorded in [VALIDATION.md](VALIDATION.md).

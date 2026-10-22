---
name: frontend-experience
description: Choose visual directions for new websites and audit or incrementally refine existing websites while preserving their identity and behavior.
---

# Frontend Experience

Use this skill when a website needs a clear visual direction, a cohesive page experience, or a design-focused audit. It guides visual decisions and frontend refinement; it does not replace product requirements, design-system ownership, or backend architecture.

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
- Use `$engineering-workflow` for substantial repository implementation, proportional planning, verification, and diff review.
- Use `$review-agent` when an evidence-based review against approved requirements is requested or useful.
- Use `$testing-engineering` to select meaningful behavior, responsive, accessibility, or visual checks; do not treat screenshots or automated checks as proof of every user experience.
- Apply `$code-documentation` when implementation comments materially clarify non-obvious behavior.

Read the style library only when choosing or comparing visual directions. For an existing site, begin with its actual pages and identity rather than applying the catalog as a redesign checklist.

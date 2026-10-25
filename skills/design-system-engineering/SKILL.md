---
name: design-system-engineering
description: Plan, implement, maintain, and review consistent accessible interfaces and design systems across web and mobile applications.
---

# Design System Engineering

Use this skill when creating or changing shared visual foundations, reusable UI components, accessibility behavior, or design-to-code mappings. It applies to new products and incremental work in established projects. Its principles are framework-independent; the detailed examples focus on React, Next.js, TypeScript, Tailwind CSS, React Native, Expo, and Figma workflows.

## Working approach

1. Inspect the current application, design files, component inventory, tokens, conventions, and available verification tools before proposing a system or change.
2. Establish the user's goals, target platforms, supported viewport/device ranges, existing visual identity, accessibility needs, and relevant design references. Ask before making a choice that would materially change the product's identity when the answer cannot be inferred.
3. Choose a proportionate change. Reuse established tokens and components; add only components or abstractions with a clear responsibility. For an existing app, prefer incremental adoption and preserve its conventions.
4. Define behavior and states alongside appearance: default, hover/focus, active, disabled, loading, empty, error, and success as applicable.
5. Implement responsive, accessible UI. Keep web semantics and React Native's native accessibility APIs distinct; shared design principles do not make platform components interchangeable.
6. Verify with the tools available for the task. Separate code checks, automated accessibility checks, device/browser inspection, and visual comparison. Never claim pixel-perfect matching without a comparison against the source at relevant sizes.
7. Explain material choices in clear everyday English and report what was actually verified.

## Design rules

- Preserve established brand and visual identity unless redesign is requested.
- Prefer semantic design tokens over repeated literal values. Reuse existing token names and themes where practical.
- Build accessible behavior into components: meaningful semantics, keyboard or native interaction, visible focus, readable contrast, labels, and status announcements where needed.
- Use simple component APIs and composition. Do not build a generic component framework for a small set of concrete needs.
- Check desktop, tablet, and mobile layouts where those sizes are in scope; consider content reflow, zoom/text scaling, touch targets, and safe areas.
- Do not duplicate a component or token until checking whether an existing one can meet the need without becoming confusing.
- Keep loading, empty, error, and success feedback consistent and understandable.
- Do not add dependencies or styling systems without a clear benefit.
- Distinguish observed design specifications from assumptions. Record unresolved states or ambiguities rather than quietly inventing product behavior.

## References

Read only the guidance relevant to the current task:

- [Foundations and tokens](references/foundations.md) for color, type, spacing, themes, layout, imagery, and visual hierarchy.
- [Components and accessibility](references/components-accessibility.md) for component APIs, states, semantics, WCAG, and interaction behavior.
- [Web and mobile implementation](references/web-mobile.md) for React/Next.js/Tailwind and React Native/Expo patterns.
- [Figma workflow](references/figma-workflow.md) when inspecting Figma or translating a design into code.
- [Maintenance and verification](references/maintenance-verification.md) for adoption, governance, test selection, and review.
- [Scenario review](VALIDATION.md) for the five design exercises used to check this skill's guidance.

## Skill boundaries

- Use `$engineering-workflow` for phased repository work, implementation, checks, documentation, and Git checkpoints.
- Use `$frontend-experience` for visual direction and its React/Next.js application-structure reference; this skill owns reusable visual foundations, component variants, accessibility, and design-system maintenance.
- Use `$backend-architecture` for backend modules, APIs, persistence boundaries, workers, and deployment architecture.
- Use `$security-engineering` for application and platform security controls; follow its guidance for sensitive UI flows and data exposure.
- Use `$debugging-workflow` when diagnosing an observed UI defect; use this skill for design-system causes only when evidence supports that connection.
- Use `$plain-english-explainer` for teaching and clear technical explanations; this skill already asks for plain-English decision summaries.
- When relevant Figma tools are connected, follow the required Figma skills before tool calls: `$figma-design-to-code` before `get_design_context`, `$figma-use` before `use_figma`, and `$figma-generate-library` when creating or maintaining a Figma component library. These are separate workflows; do not claim to have used them if unavailable.

This skill guides design and implementation. It is not a guarantee of WCAG conformance, visual fidelity, or device/browser compatibility without the corresponding checks.

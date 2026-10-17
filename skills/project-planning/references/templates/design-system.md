# Design-system plan

> **Purpose:** Record the visual and interaction rules shared across the product. **Use when:** Multiple screens/components need consistent tokens, states, or accessibility behavior. **Related:** `appflow.md` describes journeys; `prd.md` owns product behavior; `trd.md` may state platform/accessibility constraints. Use `$design-system-engineering` for detailed design and implementation.
>
> Label supplied design facts separately from assumptions and recommendations. Acceptance criteria must name the viewport/platform and observable accessibility or visual check; do not claim fidelity without comparison. Keep status `Draft` until design decisions are approved.

**Status:** Draft | **Design owner:** {{owner}} | **Platforms:** {{web/mobile/other}}

## Required sections

### Design goals and references
{{Brand/product goals, supplied Figma links/assets, and known platform constraints.}}

### Foundations
{{Existing or proposed color, type, spacing, layout, theme, and motion principles. Mark proposals clearly.}}

### Shared components and states
{{Components needed for the MVP and their important focus, disabled, loading, empty, error, or success states. Avoid speculative component inventories.}}

### Accessibility and responsive requirements
{{Semantic, keyboard/screen reader, contrast, reflow, touch, safe-area, and reduced-motion needs relevant to the target platforms.}}

### Acceptance and comparison
{{Responsive sizes/platforms, accessibility checks, and design-to-implementation comparison method. Identify source design and states to compare.}}

## Optional sections

- Token/component ownership and contribution process.
- Light/dark theme, localization, content, or platform-specific behavior.
- Figma-to-code mapping and unresolved design questions.

## Related documents and approval

{{Link app flow, requirements, and any Figma sources. Record decision owner, approval, date, and assumptions.}}

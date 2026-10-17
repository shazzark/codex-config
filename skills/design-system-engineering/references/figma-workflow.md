# Figma-to-code workflow

When a Figma file is provided or connected:

1. Inspect the relevant page, frame, component set, variants, variables, and nearby patterns before coding. Establish which viewports and states the design represents.
2. Extract intent and constraints: token values, typography, spacing, layout, responsive behavior, component variants, interaction, and assets. Separate explicit specifications from interpretation.
3. Map design components to existing implementation components first. Reuse a component if it preserves the design meaning and behavior; document a gap if it does not.
4. Identify missing focus, hover, error, loading, empty, responsive, or mobile states. Ask when an unresolved decision materially changes product identity or behavior; otherwise state a limited assumption.
5. Implement in the project's platform and styling conventions. Preserve intent while respecting semantic HTML, native platform behavior, accessibility, and performance.
6. Compare implementation and source at matching viewport sizes when screenshot or browser tools are available. Note any difference and the evidence. Do not call a result pixel-perfect without comparison.

Use the installed Figma workflow skills when available: `$figma-design-to-code` is the prerequisite before `get_design_context`; `$figma-use` is required before `use_figma`. For creating or maintaining a Figma library, use `$figma-generate-library` with its required Figma tool workflow. Follow each skill's instructions rather than duplicating its tool procedures here. If no Figma tools are available, inspect supplied exports or descriptions and state what could not be inspected.

Do not invent exact token values, breakpoints, or interaction behavior from an incomplete screenshot. Label implementation assumptions so the user can review them.

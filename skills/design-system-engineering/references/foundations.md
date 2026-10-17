# Design foundations

## Tokens and themes

Treat a design token as a named value that gives a visual choice one shared source. Use two useful levels when the project needs them:

- **Base tokens** name raw palette or scale values, such as `blue-700` or `space-4`.
- **Semantic tokens** name purpose, such as `text-primary`, `surface-raised`, `border-muted`, or `action-danger`.

Prefer semantic tokens in components so a theme can change meaningfully without changing every component. Avoid building a large token taxonomy before the product has enough repeated decisions to justify it. Keep light/dark theme values paired by semantic role and check contrast in both themes, including hover, focus, disabled, and status colors.

## Color and contrast

Use color to express hierarchy and state, not as the only way to communicate meaning. Pair status colors with text, icons, or shape. Measure foreground/background contrast rather than relying on visual intuition. As a web baseline, check the applicable WCAG 2.2 success criteria and target conformance level; requirements may also come from law, procurement, or the product's own policy. WCAG does not cover every user's needs, so usability checks still matter. Refer to the [W3C WCAG 2.2 recommendation](https://www.w3.org/TR/WCAG22/).

## Type, spacing, and layout

- Use a small, intentional type scale with distinct roles for body, label, heading, and supporting text. Check line length, line height, font loading, text scaling, and localization expansion.
- Use a spacing scale to keep repeated gaps consistent, but allow exceptions when they improve content grouping or touch usability.
- Use layout grids and alignment rules that support the content. Define breakpoints from layout needs instead of assuming a device category maps to one fixed width.
- Check narrow widths, zoom, long strings, and text resizing; avoid fixed heights that clip translated or enlarged text.

## Shape, elevation, icons, and imagery

Use radius and shadow/elevation to express consistent grouping and layering. Avoid relying on subtle shadows alone to communicate boundaries. Pick a consistent icon family and size system; give meaningful images text alternatives and decorative images empty alternatives on the web. Keep asset licensing and brand usage conventions visible to implementers.

## Hierarchy and responsive intent

Make the most important action and content easiest to find. Use size, spacing, placement, and contrast consistently. Define what rearranges, wraps, collapses, or disappears at smaller widths; preserve access to essential content and actions. A responsive layout should adapt to available space and content, not merely imitate a single screenshot.

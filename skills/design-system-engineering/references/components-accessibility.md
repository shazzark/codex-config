# Components and accessibility

## Component boundaries and APIs

Create a shared component when multiple screens need the same behavior or when one accessible implementation prevents meaningful repeated mistakes. Give each component one clear job. Prefer composition for content variation; use named variants for stable, intentional differences such as `primary`, `secondary`, or `danger`. Avoid boolean prop combinations that permit contradictory appearances or behavior.

In TypeScript, make required content and event contracts explicit. Keep DOM/native-specific props and refs appropriately typed. Do not expose styling escape hatches that bypass tokens without a demonstrated use case. Document examples that show the intended composition and important constraints.

## Common components and states

For buttons, inputs, forms, cards, dialogs, navigation, tables, and feedback components, specify purpose, interaction, responsive behavior, and states. Consider:

- default, hover, focus-visible, pressed/active, disabled, and loading;
- validation messages, success confirmation, empty results, and recoverable errors;
- long labels, narrow viewports, keyboard navigation, and screen-reader output.

For dialogs and menus, define focus entry, focus containment when appropriate, escape/close behavior, and focus restoration. For forms, associate labels and error text with fields, explain how to fix errors, preserve entered data after recoverable errors, and do not rely on color alone.

## Accessibility checks

Use semantic HTML and native controls on web where they fit. Add ARIA only when native semantics are insufficient; incorrect ARIA can make controls less understandable. Provide a logical heading order, landmarks, descriptive link names, keyboard access, visible focus, and sensible focus management. Ensure status changes are announced without moving focus unnecessarily. Respect reduced-motion preferences and avoid motion that blocks or distracts from tasks.

Use WCAG 2.2 as a web reference and confirm the applicable conformance target. Automated scans catch only some issues; include keyboard and assistive-technology checks when relevant. For mobile, expose labels, roles, states, and actions through React Native accessibility props and verify with VoiceOver/TalkBack where available. Native focus and gesture behavior may differ by platform. The [React Native accessibility guide](https://reactnative.dev/docs/accessibility) is a platform reference.

## UX safety and feedback

Make destructive or irreversible actions clear and provide a reasonable chance to review or recover. Prevent avoidable input errors with clear constraints and inline guidance. Confirm success when the user needs to know an action completed; explain failures in actionable, non-blaming language. Keep focus order and target size practical for keyboard, touch, switch, and assistive-technology users.

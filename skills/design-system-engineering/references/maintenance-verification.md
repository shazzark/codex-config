# Maintenance and verification

## Maintaining an existing system

Keep a component inventory that records purpose, owner/location, supported variants, accessibility behavior, and known limitations when the project's size makes that useful. Search for repeated patterns before adding components. Consolidate duplicates only when behavior and intent are actually the same; similar appearance alone may hide different product needs.

Manage tokens and component changes incrementally. Explain breaking API or visual changes, provide migration examples when consumers are affected, and avoid versioning ceremony for an internal app that does not need it. Introduce shared components gradually in existing applications and allow local exceptions when they represent a real product difference.

## Verification selection

Choose checks that match the change and environment:

- **Component tests:** meaningful variants, interaction, focus, validation, and state transitions.
- **Accessibility checks:** lint/static checks and automated scans, followed by keyboard or assistive-technology review for interaction-heavy changes. Automated results are partial evidence.
- **Responsive checks:** test representative narrow, medium, and wide layouts; include zoom/text scaling and long content when relevant.
- **Visual regression:** compare snapshots on stable environments; investigate intended and unintended differences.
- **Cross-browser/device checks:** verify target browsers and iOS/Android when supported and available.
- **Design comparison:** compare source and rendered output at the same viewport, content, and state. Report the actual comparison method.
- **Performance:** measure bundle/client JavaScript, render behavior, and image/font cost when changes could affect them.

Do not report a check that was not run. Distinguish test tooling output from manual inspection and design reasoning.

## Review checklist

- Did the change inspect and preserve the existing visual language?
- Does every new file, component, token, or abstraction have a meaningful responsibility?
- Are tokens semantic and reused where appropriate? Are duplicate components avoided?
- Are responsive, interactive, and non-happy-path states covered?
- Are web and native semantics implemented separately where necessary?
- Is keyboard, screen-reader, contrast, reduced-motion, touch, and focus behavior considered for the affected components?
- Are implementation assumptions and verification limits clear?

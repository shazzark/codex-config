# Design-system skill scenario review

These are design exercises used to check whether the instructions lead to proportionate, maintainable decisions. They are not executable component tests, browser runs, device checks, or visual comparisons. No application was implemented or tested during these reviews.

| Scenario | Expected guidance and review result | Check type |
|---|---|---|
| New Next.js application | Start with a small semantic token set and a focused component inventory driven by actual screens. Use TypeScript props, accessible semantic HTML, responsive layouts, and server components by default; add client components only for necessary interaction. Avoid a large UI framework without a requirement. | Design exercise; no build or browser test |
| Existing React/Tailwind app, consistency improvements | Inventory current tokens/components and repeated patterns first. Preserve brand choices and configuration, then consolidate only genuinely shared patterns in small, reviewable increments. Avoid parallel tokens and redesign. | Design exercise; no source app inspected or changed |
| Figma components to reusable React components | Inspect component sets, variants, variables, states, and responsive frames; map to existing components before creating new ones. Mark missing states and inferred details as assumptions; compare at equivalent viewport/state when tools are available. | Design exercise; no Figma file or visual comparison supplied |
| React Native/Expo screens | Reuse visual tokens and intent while using native components, safe areas, platform behavior, text scaling, touch sizes, and accessibility APIs. Verify iOS and Android separately where both are targets. | Design exercise; no emulator/device tests |
| Audit interface for accessibility, responsiveness, and duplicate components | Inspect semantics, keyboard/focus, labels, contrast, motion, content reflow, target sizes, component inventory, and repeated patterns. Combine automated checks with manual assistive-technology and responsive review; report only checks actually performed. | Design exercise; no app or automated scans supplied |

## Findings and limitations

The guidance yields clear boundaries: design foundations and shared UI behavior belong here; backend architecture, security control design, and root-cause investigation remain with their respective skills. It makes platform differences and validation evidence explicit. The scenarios do not establish WCAG conformance, implementation quality, runtime compatibility, or pixel accuracy. Those require the actual design, codebase, target platforms, and appropriate checks.

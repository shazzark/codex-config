# Web and mobile implementation

## React, Next.js, TypeScript, and Tailwind CSS

- Follow the repository's existing framework version, routing model, component boundaries, naming, and styling conventions.
- Keep reusable UI components focused on rendering and interaction. Keep product-specific orchestration near the feature that owns it.
- In Next.js, preserve the existing App Router or Pages Router conventions. With the App Router, keep components server-rendered by default where possible; add a client boundary only for browser state, event handlers, or APIs that require it. Do not move large subtrees to the client for a small interactive control.
- Define design tokens through the project's existing mechanism. CSS custom properties work well for semantic tokens and theme switching; connect Tailwind theme utilities to these tokens where the existing configuration supports it. Avoid scattering arbitrary values or creating a parallel token system.
- Use responsive utilities for layout changes and semantic classes/components for repeated visual patterns. Extract components when reuse, semantics, or interaction complexity warrants it, not just to shorten a JSX file.
- Keep asset loading, font strategy, animation, and client JavaScript proportionate. Avoid adding heavyweight UI libraries when a focused existing component is sufficient.

## React Native and Expo

React Native renders native platform UI primitives; web CSS, HTML semantics, and browser breakpoints do not transfer directly. Reuse design decisions such as token meaning, hierarchy, and component intent, but implement with native layout, text, accessibility, and navigation APIs.

- Check iOS and Android behavior separately when both are supported; platform conventions and built-in controls differ.
- Account for safe areas, device dimensions, orientation, touch targets, keyboard avoidance, and dynamic text size.
- Use accessibility labels, roles, states, and actions that describe the control's purpose. Check VoiceOver and TalkBack when devices or emulators are available.
- Expo packages and platform capabilities vary by SDK version; inspect the project's installed versions and current official documentation before relying on a specific API.
- Keep gestures discoverable and provide accessible alternatives for actions that otherwise require a gesture.

## Cross-platform reuse

Share stable design tokens and domain-level content/behavior when it reduces drift. Do not force web and native into one component API when their accessibility, navigation, layout, or interaction models differ. A small shared token source with platform-specific components is often easier to maintain than a universal UI abstraction.

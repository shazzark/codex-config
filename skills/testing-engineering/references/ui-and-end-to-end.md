# UI and end-to-end testing

## React and Next.js

Prefer tests that interact as a user would: locate controls by accessible role/name/label, perform an interaction, then assert visible content, state, or navigation. React Testing Library encourages this style; avoid testing component internals or hook implementation unless the module itself has a meaningful isolated contract. Use `user-event` or the project's equivalent for realistic DOM interaction.

For Next.js, inspect App Router/Pages Router conventions and use the appropriate framework test setup. Unit/component tests can verify focused client behavior; server rendering, route handlers, middleware, server actions, and framework caching may require integration or E2E coverage. Mock only framework boundaries that do not need to be verified in the given test. Consult [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) and [Next.js testing](https://nextjs.org/docs/app/guides/testing).

## Playwright and user journeys

Use Playwright for a small set of high-value browser flows across a running app: sign in, complete a core workflow, and observe success or a meaningful error. Prefer role/label/text locators and user-facing outcomes over CSS selectors or internal implementation hooks. Make test data isolated and cleanup reliable. Avoid fixed sleeps; wait for a real state or bounded condition. Cover authentication and authorization journeys with valid and denied users where appropriate, without using production identities.

Test representative responsive widths and important browser behavior. Visual regression tests compare rendered screenshots; they need stable data, fonts, viewport, browser, and environment. Review diffs because screenshot changes can be intentional or environmental. Neither a snapshot nor an automated accessibility scan proves full accessibility.

## Accessibility and mobile

Include automated accessibility checks where configured, then manually review keyboard navigation, focus, accessible names, error announcements, and relevant assistive technology for high-impact flows. Automated rules catch only some accessibility barriers.

React Native components use native accessibility APIs rather than DOM semantics. Test component behavior with the project-supported React Native testing tools, but also run on Android/iOS simulators or devices when platform-specific focus, gestures, keyboard, safe areas, permissions, or screen readers matter. Expo Go, development builds, simulators, and physical devices may expose different native capabilities; state which were actually used. Do not claim cross-platform behavior from a single platform run.

Useful primary references: [Playwright](https://playwright.dev/docs/intro), [React Native testing overview](https://reactnative.dev/docs/testing-overview), and the project's current Expo testing guidance. Inspect installed versions and tooling before changing setup.

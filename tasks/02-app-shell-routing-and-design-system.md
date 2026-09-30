# Task 02 — App shell, routing, and design system

## Context and dependencies
Tasks 00 and 01 are complete. The root Vite/React/TypeScript project exists. Implement the approved design artifacts in design/ for the private LMS, without adding business data yet.

## Work
Create route placeholders for sign-in, dashboard, course overview, course builder, chapter reader, chapter test, flashcards, downloads, and not-found. Add protected-route and public-route boundaries that Task 05 can connect to Supabase Auth. Implement shared navigation, page headers, responsive layout, buttons, forms, dialog, toast/status message, skeleton, and empty/error components. Add design tokens as CSS custom properties and responsive styles from Task 01; use semantic HTML and visible focus states. Support browser back/forward and direct route reloads. Add a global boundary for unexpected render errors.

## Verification
Run build, typecheck, lint, and focused component tests for navigation and route fallbacks. Inspect 390 px and 1440 px widths. Check keyboard access to main navigation and dialog close. No route should crash when loaded directly.

## Done when
All primary routes render a coherent, responsive shell consistent with the mockups, with accessible placeholders ready for feature work.

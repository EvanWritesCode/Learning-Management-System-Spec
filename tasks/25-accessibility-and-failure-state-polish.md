# Task 25 — Accessibility, resilience, and security polish

## Context and dependencies
Tasks 02–24 implement the product. Task 01 defines design and accessibility targets. This task closes concrete cross-feature gaps before release testing; it does not add new product scope.

## Work
Audit forms, navigation, reader, PDF controls, dialogs, tests, cards, and downloads for keyboard use, visible focus, labels, error messages, contrast, touch targets, reduced motion, and screen-reader announcements. Audit loading, empty, offline, expired-session, unavailable-embed, oversized-file, and paused-backend states. Review Markdown sanitization, HTTPS URL validation, private Storage access, and absence of secret keys in built assets. Fix issues found. Add a recoverable error boundary and retry actions where appropriate. Do not auto-open external content without user intent.

## Verification
Use automated accessibility checks where useful, then manually navigate the full happy path and error path using keyboard and a screen reader. Inspect 390 px and 1440 px layouts, zoomed text, and Android TalkBack if available. Re-run security policy and sanitizer tests affected by fixes.

## Done when
A learner can complete all main flows with accessible controls and understandable failure recovery, and no known high-impact privacy or injection issue remains.

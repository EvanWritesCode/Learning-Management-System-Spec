# Task 01 — UI/UX design, wireframes, and mockups

## Context and dependencies
Design the private, self-directed learning app described in ../IMPLEMENTATION_PLAN.md before implementing product screens. This task can start while Task 00 local setup is in progress; hosted accounts and device access are not dependencies. There are two principal modes in one account: building a course and studying it. The product is reading-heavy, supports chapters, PDFs, videos, articles, tests, flashcards, progress, and Android offline downloads. Do not require a Figma account or subscription.

## Work
1. Define information architecture and navigation for authentication, dashboard, course overview, course/chapter builder, import review, material reader, test, attempt review, flashcard queue, and downloads.
2. Map user flows for: create a course and chapter; import externally generated chapter JSON and resolve PDF placeholders; publish; resume reading; open a blocked article; pass and retake a test; review cards; download a chapter and resume offline.
3. Make low-fidelity wireframes for desktop (1440 px) and Android-sized mobile (390 px) for every principal screen. Show content hierarchy, navigation, empty/loading/error states, long text, PDF controls, embedded media, and the visible external-page fallback.
4. Create high-fidelity mockups for at least dashboard, course overview, reader, builder, test, and flashcards on both widths. Use a restrained reading-first visual system with readable line length and type scale. Define colors, spacing, typography, focus states, touch targets, icon usage, and responsive breakpoints.
5. Produce a component inventory and state matrix (default, loading, empty, validation error, offline, complete) so coding agents can build consistent UI. Include rejected published edits, threshold-change retest messaging, pending PDF deletion/cleanup retry, offline reading after token expiry, definitive-session-rejection reauthentication, and interrupted sign-out cleanup. Document keyboard paths, labels, contrast, reduced motion, and screen-reader announcements. Prefer WCAG 2.2 AA as the acceptance target.
6. Save design artifacts in design/: flows.md, design-system.md, component-inventory.md, wireframes/, and mockups/. Use SVG or PNG assets that can be inspected without a proprietary tool. If a clickable HTML prototype helps, keep it in design/prototype/ and do not treat it as production code.

## Verification
Visually inspect every exported wireframe and mockup at native size. Walk through each flow from start to finish and confirm all actions and fallback states have a destination. Review desktop and mobile layouts for long lesson titles, long text, and a 50 MB PDF upload error. Check contrast and keyboard/focus specifications.

## Done when
Another agent can implement the screens without choosing a new navigation model or visual language. All named artifacts exist, match the product scope, and include desktop/mobile designs plus explicit accessibility and error states.

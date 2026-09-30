# Task 15 — Chapter reader navigation and authored text

## Context and dependencies
Tasks 06, 10, and 14 provide course overview, safe authored Markdown, and published chapters. This task creates the learner's main reading surface. Progress persistence follows in Task 18.

## Work
Implement a chapter outline/sidebar or mobile drawer with objectives, ordered materials, test, and flashcards. Allow any chapter in the course to open; do not lock later chapters. Show next/previous material actions, a breadcrumb/back path, and the selected material's title and type. Render authored Markdown with the same sanitized GFM, code, table, and math pipeline as the editor. Match Task 01 reading typography and responsive mockups. Show a useful empty/removed-material state and never expose draft content through normal study routes.

Exclude pending-deletion content and descendants from online reader navigation; direct links show deletion-in-progress rather than fetching a removed PDF. Tasks 23–24 provide owner-bound cached navigation for offline downloads independently of online queries and token freshness.

## Verification
Navigate a multi-chapter course by pointer, keyboard, browser back/forward, and direct URL. Test very long text, headings, code overflow, tables on narrow screens, and sanitized links. Confirm another user's IDs do not display content.

## Done when
A learner can read published text in chapter order or jump freely while retaining a clear sense of location.

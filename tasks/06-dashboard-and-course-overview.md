# Task 06 — Dashboard and course overview

## Context and dependencies
Tasks 02, 03, 04, and 05 provide the responsive shell, data model, security, and authentication. The dashboard is the signed-in user's entry point for private courses; chapter completion is derived later in Task 18.

## Work
Build a dashboard query and UI listing only the current user's courses. Show title, description, draft/published state, chapter count, latest activity, and a provisional progress area that becomes computed completion in Task 18. Add empty, loading, error, and paused-backend states. Build a course overview with ordered chapters, objectives/summary, material counts and types, and actions to study, edit, or create content. Show drafts only to their owner and distinguish preview from published study. Provide a clear Resume action that uses last-opened progress when Task 18 supplies it; until then route to the first published material. Keep queries narrow and indexed.

When Task 12 adds pending deletion, exclude pending targets and descendants from study/resume/counts and expose their deletion status and retry action in management views. Never present a target awaiting file cleanup as an active course/material or as already deleted.

## Verification
Test empty and populated dashboards, a course with no chapters, drafts versus published chapters, direct route reload, and backend failure. In two-user tests, each user sees only their own courses. Inspect mobile and desktop layouts against design/ mockups.

## Done when
A signed-in user can discover and open their private courses and chapters from responsive dashboard and overview screens, with all states understandable.

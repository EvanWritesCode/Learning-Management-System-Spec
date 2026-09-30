# Task 26 — Integrated regression and security tests

## Context and dependencies
Tasks 03–25 implement and polish the complete app. Individual tasks already have focused tests; this task verifies their integration and adds only tests for risks that cross feature boundaries.

## Work
Create browser end-to-end scenarios for sign-up/sign-in, course creation, chapter JSON import, PDF upload, publish, long-form reading, external fallback, pass/retake test, attempt review, flashcards, and resume. Seed deterministic local data through migrations/fixtures rather than production credentials. Run two-user RLS/Storage policy scenarios and cross-user route checks. Include invalid JSON, failed upload, blocked iframe, chapter revision, and auth-expiry recovery. Add a documented Android manual smoke checklist for offline download/sync and native browser behavior. Ensure tests do not mutate the hosted production project.

Cover the shared contracts from [the execution guide](README.md): threshold-only revision and historical threshold display; stale test submission racing an edit; invalid published child edits and concurrent last-item deletion; direct attempt UPDATE/DELETE denial with allowed owner-parent cascades; interrupted PDF upload/replacement/deletion at each persisted phase, lost responses, and cleanup resumption without deleting live objects. Assert pending-deletion content is excluded consistently from dashboard, reader, and completion. Explicitly configure test servers/builds to use local Supabase even when an ignored .env.production.local exists for release work.

Execute the Android smoke checklist using the existing Task 02a/22 project on API 28 and a current Android version. Include storage isolation, authentication, a near-limit PDF, and real external media. Download text/PDF, disconnect through access-token expiry, kill/relaunch, read and queue progress, then reconnect through same-owner session validation. Verify definitive-session-rejection recovery, offline sign-out interrupted by restart, filesystem cleanup failure/retry, and different-user access only after cleanup. Record completed results before release; an unexecuted checklist does not satisfy native acceptance.

## Verification
Run typecheck, lint, unit tests, database policy tests, local end-to-end tests, and production build from a clean checkout. Record exact commands and results. Repair failures; do not weaken tests to make the suite pass.

## Done when
A clean checkout reproduces all critical browser and database checks, and the Android smoke checklist has passed against local test data on the minimum and current Android versions. Task 28 repeats production-backend acceptance on the release APK.

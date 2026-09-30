# Task 03 — Database schema and reproducible migrations

## Context and dependencies
Task 00 established the local Supabase CLI/Docker stack. The data model is specified in ../IMPLEMENTATION_PLAN.md. This task defines data only; access policies follow in Task 04.

## Work
Create tracked SQL migrations for courses, chapters, materials, questions, flashcards, material_progress, test_attempts, card_reviews, and file_operations. Use UUID keys, foreign keys with appropriate deletion behavior, nonnegative ordered positions, created/updated timestamps, question/material type constraints, passing percentage between 0 and 100, and indexes for owner/course/chapter and due-review queries. Keep questions' type-specific answer data in validated JSON where relational columns add no value. Store test-attempt question snapshots and pass_percent_snapshot so later question or threshold edits do not change historical results. Submitted attempts cannot be updated or individually deleted by application roles; deleting their owning chapter/course may remove them through the controlled parent-deletion flow. Enforce update immutability in the database and coordinate operation-specific grants with Task 04 rather than using a blanket delete trigger that breaks parent cascades.

Represent chapter content revision explicitly. Give each material a content_revision starting at 1 and record the revision last marked complete in material_progress, so edited reading requires a new completion mark while unchanged reading retains its mark. Reserve lifecycle states for pending deletion of courses, chapters, and materials. file_operations durably records the owner, operation ID/type/state, target, exact old/new object paths, retry/error information, and timestamps before Storage side effects. It must survive target deletion until cleanup is confirmed; do not cascade away outstanding jobs. Use unique object paths for PDF replacements. Task 12 implements the recoverable lifecycle; Task 14 implements transactional readiness and revision rules, including passPercent changes. Do not create a redundant writable chapter-completion flag; completion is derived from current material progress and passing attempts. Create minimal seed data for local development without real user content.

## Verification
Run local database reset from a clean state and inspect every table, constraint, and index. Add migration tests for invalid pass percentage, nonexistent parent IDs, threshold snapshot persistence, deletion cascades, immutable-attempt behavior, and file-operation survival after target deletion. After Task 04, verify direct attempt UPDATE/DELETE are denied but authorized parent deletion can cascade. Confirm applying migrations twice via normal migration tracking does not duplicate data.

## Done when
A fresh local Supabase instance can reproduce the full schema solely from tracked migrations and seed files, with no dashboard-only schema edits.

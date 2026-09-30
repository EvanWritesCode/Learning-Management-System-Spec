# Task 21 — Flashcard review and due queue

## Context and dependencies
Tasks 03 and 14 provide review records and authored cards. Cards are for recall practice and do not gate chapter completion. Reviews require connectivity in the first release.

## Work
Build chapter-level card study and course-wide due queue. New cards are due immediately. Show front, reveal back, then offer Again and Remembered; do not allow grading before reveal. Use five boxes: Remembered advances to 1, 2, 4, 8, then 16 days; Again resets and repeats in the current session. Persist box, due date, and last review. Prevent duplicate reviews caused by rapid taps. Handle deleted or edited cards without leaking stale review state. Show due counts on course overview.

## Verification
Unit-test the schedule across all boxes, repeat/reset, date boundaries, and a newly created card. UI-test reveal, keyboard controls, empty queue, offline state, and persistence after reload. Confirm one user's review records are inaccessible to another.

## Done when
Learners can practice authored cards and return to a predictable due-review queue.

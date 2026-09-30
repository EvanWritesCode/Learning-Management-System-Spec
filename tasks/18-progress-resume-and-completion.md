# Task 18 — Reading progress, resume, and completion

## Context and dependencies
Tasks 03, 06, 14, 15, and 16 provide progress tables, dashboard, published revision rules, text reader, and PDF viewer. Test attempts from Task 19 will later determine the pass portion of completion.

## Work
Persist the last-opened material, Markdown heading/scroll offset, PDF page, and explicit material-complete state for the signed-in user. Store the material's content_revision with each completion mark. Restore reading position on return and provide a Resume action on dashboard/course overview. Keep completion monotonic for a given material revision: opening or revisiting it cannot undo a completed mark. Derive current chapter completion from all current materials completed at their current content_revision plus a passing attempt for the current chapter revision; derive course completion from its published chapters. Before Task 19 exists, display the test requirement as pending rather than treating reading alone as complete. On a published revision change, surface which changed materials need a new completion mark and that a fresh test pass is needed. Use the owner's timezone only for display; store instants consistently.

## Verification
Read several materials across two browser sessions and confirm resume location and material checks persist. Test empty chapters, newly added materials, a revised chapter, and a previously completed chapter. Test a threshold-only revision: reading marks stay complete but a fresh current-revision test pass is required. Exclude pending-deletion content consistently with Task 12. Verify dashboard percentages and next unfinished chapter update when Task 19 supplies attempts.

## Done when
The app reliably resumes study and reports completion according to the agreed read-and-pass rule, without a writable completion shortcut.

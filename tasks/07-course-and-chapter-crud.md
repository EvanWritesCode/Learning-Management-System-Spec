# Task 07 — Course and chapter creation and editing

## Context and dependencies
Tasks 03–06 provide schema, RLS, auth, and overview UI. The same signed-in person authors and studies private courses. PDF handling is added in Task 12; publishing rules in Task 14.

## Work
Implement forms to create and edit course title/description and chapter title/summary/objectives/pass percentage. Preserve objective keys once referenced by questions; show an error before removing a referenced objective. Add chapter reordering with keyboard-accessible controls and stable stored positions. Keep new courses/chapters as drafts. Provide confirmation for deletion; allow full deletion of content without PDFs now, and connect file-backed cleanup when Task 12 lands. A failed write must leave the editor open with entered values intact. Use optimistic UI only if rollback on error is explicit.

Explain that parent deletion also removes its attempts through the authorized cascade; do not add individual attempt deletion. Task 12 replaces file-backed deletion with durable pending/cleanup/finalize states. Task 14 must route published objective/passPercent edits through atomic readiness and revision enforcement; do not leave a direct update path that bypasses those rules. Pending-deletion targets and their descendants cannot be edited.

## Verification
Create, edit, reorder, and delete courses and chapters; reload to confirm persistence. Test blank title, invalid pass percentage, duplicate objective keys, and failed writes. Verify another user cannot edit via a forged ID. Test reorder by keyboard and pointer.

## Done when
The owner can manage draft course and chapter structure without corrupting order, losing form input, or crossing ownership boundaries.

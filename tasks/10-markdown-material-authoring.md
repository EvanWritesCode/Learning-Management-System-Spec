# Task 10 — Long-form Markdown material editor

## Context and dependencies
Tasks 07 and 08 define chapters and material fields. Long-form locally authored text is a primary learning format, so authoring and preview must work well for lengthy content.

## Work
Add create/edit/delete/reorder controls for markdown materials in a chapter draft. Build a Markdown editor with preview, headings, tables, fenced code, links, and math rendering. Sanitize rendered HTML, disallow arbitrary raw HTML, and show readable line length, typography, and heading navigation from the Task 01 design. Save drafts without silently truncating large content. Keep stable material IDs on edits so progress can attach later. Validate nonempty title/body and safe links. Warn before deleting a material that already has reading progress.

Task 14 extends these controls to published chapters through its guarded transaction: enforce readiness, reject last-material deletion, and increment chapter/material revisions as appropriate. Failed published saves keep the previous content and editor input. Do not implement a separate mutation path that bypasses these rules.

## Verification
Save/reload a long lesson with headings, table, code, math, and links. Test malicious HTML and script URLs are not executable. Test reordering and failed-save recovery. Inspect mobile editing and preview for overflow and keyboard use.

## Done when
The owner can author substantial text inside a chapter and see a safe, faithful preview that the reader can reuse.

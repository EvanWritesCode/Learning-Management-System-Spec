# Task 11 — External article and video material authoring

## Context and dependencies
Tasks 07 and 08 provide chapter editing and the material contract. Articles and videos are URL references; no scraping or video upload is allowed. Rendering follows in Task 17.

## Work
Add forms to create/edit/delete/reorder article and video materials. Accept HTTPS only. Normalize YouTube watch, short, and embed links to a video ID; normalize Vimeo links to an ID. Preserve the original source URL for attribution and fallback. For other video hosts, store the HTTPS URL but classify it for external-view fallback unless a safe supported embed is available. Show title, source hostname, and a preview/fallback explanation. Never allow user-provided iframe HTML or JavaScript. Ensure URL parsing cannot treat lookalike hosts or javascript/data URLs as approved providers.

When Task 14 enables published editing, use its guarded readiness/revision transaction for URL changes, additions, and deletions. Reject deletion of the last published material, and preserve editor input and existing content when a save fails.

## Verification
Test valid YouTube/Vimeo forms, generic HTTPS article/video URLs, malformed URLs, non-HTTPS schemes, spoofed YouTube hostnames, and query strings. Save/reload and reorder materials. Verify no arbitrary embed code enters persisted content.

## Done when
A creator can add external reading and video resources with safe, predictable URL metadata for the reader.

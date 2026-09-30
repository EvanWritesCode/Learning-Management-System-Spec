# Task 16 — Inline PDF viewer

## Context and dependencies
Tasks 12 and 15 provide private uploaded PDFs and reader navigation. Task 02a provides the Android shell for immediate native verification. PDFs must display within the website and Android app, including downloaded Android copies later. Use PDF.js rather than relying on a browser's built-in PDF plugin.

## Work
Fetch a private PDF with the signed-in user's access, feed its bytes to PDF.js, and render pages lazily. Provide page navigation, page count, zoom/fit width, loading, retry, and accessible controls. Avoid leaving long-lived signed URLs in persisted state. Expose current page and page-change events for Task 18. Accept a local byte source so Task 23 can reuse the same viewer offline. Keep memory bounded for PDFs near the 50 MB limit; release PDF.js resources when leaving the material.

## Verification
View small, multi-page, and near-limit PDFs in desktop/mobile browsers and the Task 02a Android WebView now, including API 28; do not defer the first device check to Task 22. Test expired URL/session, corrupt PDF, rapid page changes, direct reload, and pending-deletion content. Confirm the PDF cannot be fetched by another user through a guessed object path.

## Done when
Private PDFs remain inside the course reader with reliable navigation and a reusable offline input path.

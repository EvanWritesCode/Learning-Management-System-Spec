# Task 17 — External video and article viewing

## Context and dependencies
Tasks 11 and 15 provide validated external URLs and the reader shell; Task 02a already provides the native browser adapter. The user wants content inline wherever possible, with an in-app fallback when embedding is prohibited.

## Work
Render YouTube and Vimeo through their documented embed URLs at responsive, accessible sizes. For other video hosts, try only a safe compatible HTTPS embed when its provider permits it; otherwise present the external-view action. Attempt article display in an iframe with a visible source label and **Open in app view** control. Do not scrape, proxy, or silently copy article content. On web, the fallback opens a new tab without changing the course route. On Android, use the isolated Capacitor InAppBrowser adapter from Task 02a and verify the real material return path in this task. Show an explicit explanation if a source cannot play or display. Do not promise automatic detection of all iframe blocks because cross-origin policies may prevent it.

## Verification
Test a working YouTube embed, a non-embeddable video, an embeddable article, and an iframe-blocked article. Check the fallback and return path on web and Android. Verify unsafe URL schemes and hostile iframe HTML never run.

## Done when
External resources display inline when permitted and always have a usable, safe fallback.

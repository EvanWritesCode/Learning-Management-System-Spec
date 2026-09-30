# Task 23 — Android offline chapter downloads and reading

## Context and dependencies
Tasks 05, 16, and 22 provide the offline-access contract, a PDF viewer that accepts bytes, and a verified Android shell originally created in Task 02a. The first release supports explicit offline downloads of authored Markdown and uploaded PDFs only. External articles, videos, tests, and card reviews remain online.

## Work
Add a Download chapter control, status/progress display, retry/remove actions, and a downloads management screen. Store chapter metadata and Markdown under a user-scoped IndexedDB key; use Capacitor File Transfer/Filesystem to download private PDFs to app-private storage via short-lived authenticated URLs. Validate ownership, file size, integrity, and available-space errors. Use the bundled app shell and locally stored navigation metadata to show downloaded text and local PDFs without waiting for network queries or token refresh. Implement Task 05's owner-bound offline access even after access-token expiry and a full app restart; a local owner binding never authorizes a backend request. Mark external materials as unavailable offline without losing reader location. Keep the cached content revision; when online, indicate that a newer published revision can be downloaded. Stage refreshes separately and switch the active cache only after all required content is verified, so interruption preserves the previous readable download. Keep auth credentials out of chapter caches and do not persist signed download URLs.

## Verification
Download a chapter containing long text and a PDF; switch off network, let the access token expire, kill/relaunch the Android app, and read both through cached navigation. Confirm no sign-in redirect or loading loop prevents reading and online-only features stay unavailable. Test canceled/failed downloads, insufficient space, corrupt PDF, removal, interrupted refresh of revised content, and wrong-user cache access. Verify cached data is inaccessible after explicit sign-out or a definitive session rejection, according to Task 05. Verify web remains online-first and does not present a misleading offline-download feature.

## Done when
Selected authored text and PDFs are readable in the Android app after a full offline restart, and cache management is clear and private.

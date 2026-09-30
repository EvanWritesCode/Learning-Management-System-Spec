# Task 24 — Offline reading progress sync and cache cleanup

## Context and dependencies
Tasks 05, 18, and 23 provide owner-bound offline access, online progress, and offline Android reading. Only reading position and material completion may be changed offline; tests and card reviews remain online.

## Work
Queue offline progress events under the locally bound owner ID with material ID, material content_revision, event type, position/completion, and timestamp. On reconnect, refresh/validate authentication and verify that the returned user ID matches the queue owner before replaying idempotently against owner-scoped Supabase records. Transient network/refresh failures or a paused backend preserve offline reading and queued changes. A definitive invalid/revoked session locks cache access and pauses sync pending online reauthentication by the same owner; never replay one owner's events with another user's credentials. Completion is monotonic within a material revision; an event for an older revision cannot mark current content complete or undo a newer completion. For reading position, keep the newest timestamped position. Handle retries and partial network failures without duplicate side effects. Show a sync-pending/error indicator.

On explicit sign-out, invalidate local access immediately and persist a cleanup-pending marker before removing cached chapters, PDF files, test drafts, queue, owner binding, and local session credentials. Cleanup must work without a successful remote sign-out request. If interrupted or a file removal fails, retry on restart, keep private routes locked, and block all new account sign-ins until cleanup completes. Only then clear the cleanup marker. Warn that signing out discards unsynced local progress. This local cleanup must not erase durable remote file_operations jobs from Task 12; those resume when the same owner signs in online again. If the Supabase free project is paused, retain local reading and queue unless explicitly signing out, show an online-unavailable message, and retry when the backend becomes available.

Apply the same cleanup gate before switching away from any locally bound owner, including choosing a different account after session rejection. Same-owner reauthentication may restore access to preserved data; a different account must first clear the previous owner's local data.

## Verification
Change text/PDF position and mark a material complete offline after token expiry and an app restart, reconnect, then confirm the website shows the same state after session refresh. Repeat reconnect and ensure no duplicate updates. Simulate conflicting older device events, failed sync, paused backend, definitive session rejection, same-owner reauthentication, and an attempted different-user replay. Sign out while offline, interrupt cleanup, restart, and verify cached access and new sign-ins remain blocked until every local private artifact is removed. Simulate a filesystem deletion failure and recovery. Inspect device storage and auth storage after cleanup, then sign in as a different user and confirm no old content or queue appears.

## Done when
Offline reading changes sync safely and one user's downloaded content cannot be exposed to the next signed-in user.

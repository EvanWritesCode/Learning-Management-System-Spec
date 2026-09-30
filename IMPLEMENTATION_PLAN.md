# Learning and Professional Development Navigator — Implementation Plan

## 1. Context and decisions

This is a greenfield project. `Initial_Requirements.txt` defines a learning application centered on long-form, chapter-based courses, deep knowledge checks, progress tracking, mixed media, flashcards, and a course builder. The workspace contains requirements, this plan, and the task breakdown; there is no existing application or data to migrate.

The first release is a private, self-directed product: each signed-in user creates and studies only their own courses. It includes the complete core workflow rather than a reduced reading-only MVP. Course sharing, invited learners, a public catalog, runtime LLM calls, and Play Store publication are outside this release.

The application must ship as both a responsive website and an installable Android APK. Android users can explicitly download authored text and uploaded PDFs for offline reading. Tests, flashcard reviews, external articles, and videos require connectivity. Users may open chapters in any order; the app recommends the next unfinished chapter and restores the last material opened.

Each chapter has ordered materials, learning objectives, a test, and flashcards. A learner marks materials complete. The chapter completes when all current materials are complete and a test attempt passes. The default passing score is 80%, editable for each chapter. A course completes when all its published chapters complete.

## 2. Platform evaluation and architecture

Build one React, TypeScript, and Vite application. Host its static web build on Cloudflare Pages and package the same interface for Android with Capacitor. Use Supabase Auth, Postgres, and private Storage as the backend. The client communicates directly with Supabase using its public client key; Cloudflare Pages needs no server functions.

| Need | Choice and rationale |
| --- | --- |
| Backend | **Supabase** provides relational Postgres data, authentication, and private file storage. Courses, chapters, questions, attempts, and progress are naturally related. Its free tier currently includes a 500 MB database and 1 GB of file storage, with a 50 MB per-file upload limit. Free projects can pause after inactivity; the owner must resume one before online learning or sync works again. [Pricing](https://supabase.com/pricing), [file limits](https://supabase.com/docs/guides/storage/uploads/file-limits), [pausing](https://supabase.com/docs/guides/platform/free-project-pausing). |
| Backend alternatives | Firebase has a useful no-cost Firestore allowance, but Firebase file storage requires a billing-enabled Blaze project. Appwrite offers more free file storage, but its free projects can also pause, and its model is a weaker fit for these relational records. [Firebase storage requirements](https://firebase.google.com/docs/storage/faqs-storage-changes-announced-sept-2024), [Appwrite pricing](https://appwrite.io/pricing). |
| Website hosting | **Cloudflare Pages** serves a purely static site with unlimited free static requests, subject to its build and asset limits. Firebase Hosting has a 360 MB/day no-cost transfer allowance. Vercel Hobby is restricted to personal, non-commercial use. [Cloudflare Pages](https://developers.cloudflare.com/pages/functions/routing/), [Firebase pricing](https://firebase.google.com/pricing), [Vercel Hobby](https://vercel.com/docs/plans/hobby). |
| Android | **Capacitor** reuses the web interface and provides native file and browser APIs. Use an InAppBrowser version with isolated `openInWebView`, keep `isIsolated` enabled, and set the minimum Android SDK to **28 (Android 9)**. API 26–27 cannot provide the required cookie/local-storage isolation and are outside this release. Verify API 28 and a current Android version. [Capacitor](https://capacitorjs.com/docs), [InAppBrowser isolation](https://capacitorjs.com/docs/apis/inappbrowser#localstorage-isolation). |

Use a small feature-oriented source structure: authentication, course builder, reader, assessments, flashcards, and offline downloads. Keep the chapter JSON Schema and its TypeScript validation in a shared contracts area. Keep SQL migrations and database policy tests with the Supabase configuration. Include setup, deployment, and authoring instructions in project documentation.

## 3. Data model, access, and authoring contract

Create migrations for the following records. Use UUID primary keys, foreign keys, explicit display positions, timestamps, and indexes for course ownership, ordered chapter/material lookup, attempts, and due reviews.

| Record | Required data and behavior |
| --- | --- |
| `courses` | Owner ID, title, description, draft/published status, deletion lifecycle state, timestamps. |
| `chapters` | Course ID, position, title, summary, learning objectives, passing percentage, draft/published status, deletion lifecycle state, content revision. |
| `materials` | Chapter ID, position, type, title, content revision, deletion lifecycle state, and the type-specific Markdown body, HTTPS URL, or private PDF path. |
| `questions` | Chapter ID, position, type, prompt, linked objective keys, answer data, explanation, and written-answer rubric where applicable. |
| `flashcards` | Chapter ID, position, front and back content. |
| `material_progress` | User and material IDs, completion state and completed material revision, last reading position, and last-opened time. |
| `test_attempts` | User and chapter IDs, chapter revision, submitted answers, graded-question snapshot, `pass_percent_snapshot`, score, pass result, and submission time. Attempts cannot be updated or individually deleted by application roles. Authorized parent chapter/course deletion may cascade them. |
| `card_reviews` | User and card IDs, review box, next due date, and last review time. |
| `file_operations` | Owner, operation ID/type/state, target, exact old/new object paths, retry/error information, timestamps. Persist before Storage side effects and retain until cleanup is confirmed, even if the target has been deleted. |

Enable row-level security on **every exposed table**. Authenticated users may access only rows under courses they own. Store PDFs in a private Supabase Storage bucket under a user-prefixed path and apply equivalent owner-scoped Storage policies. Do not expose the service-role key to the website or Android application. [Supabase RLS](https://supabase.com/docs/guides/database/postgres/row-level-security), [private Storage](https://supabase.com/docs/guides/storage/buckets/fundamentals).

Use operation-specific grants: attempts have no direct UPDATE or DELETE permission, and submitted updates are rejected by the database. Guarded content mutations must enforce publication and file-lifecycle rules even for direct API clients. Cleanup authorization is tied to validated owner-scoped file operations and exact paths, so it survives target deletion without permitting arbitrary live-object removal.

Publish a downloadable **version 1 chapter JSON Schema**, an example chapter, and copyable prompts for an external LLM. The top-level JSON fields are `schemaVersion`, `title`, `summary`, `objectives`, `materials`, `assessment`, and `flashcards`:

- Objectives have short, unique keys and text. Questions reference one or more objective keys.
- Materials are ordered and typed as `markdown`, `article`, `video`, or `pdf`. Markdown has a body; articles and videos have HTTPS URLs. A PDF entry is an upload placeholder; its file is selected and uploaded in the editor, never embedded as base64 in JSON.
- The assessment has `passPercent` (default 80) and ordered questions. Each question has a type, prompt, objective references, explanation, and type-specific answers.
- Flashcards have front and back content. The app assigns database IDs during import, so external authors do not need UUIDs.

The builder validates the entire import, reports field-level errors, and previews it before creating a **new draft chapter**. Importing does not overwrite an existing chapter. The draft remains editable in forms before publication. Show objective coverage in the builder and warn about objectives with no test question. Require a nonempty test to publish a chapter; do not impose an arbitrary question-count minimum.

Support four question types:

1. `single_choice`: exactly one correct option.
2. `multiple_choice`: an exact set of correct options; no partial credit.
3. `short_answer`: one or more accepted answers; compare after normalizing case and surrounding/repeated whitespace.
4. `written`: show a model answer and rubric after the learner responds, then let the learner award zero or one point.

Each question is worth one point. Calculate the percentage as points earned divided by question count, without rounding it before comparison with the chapter threshold. Allow unlimited retakes, show prior attempts, and retain the best result. Every successful logical published save that adds, removes, or changes material, question, or objective content, or changes `passPercent`, increments the chapter revision once in the same transaction. Changed materials also increment their own content revision. Threshold-only edits preserve reading marks but require a new passing attempt. Flashcard, title, position-only, failed, and no-op saves do not increment the chapter revision. Historical attempts retain their original question and threshold snapshots; completion requires all active current materials completed at their current revisions and a passing attempt for the current chapter revision.

Enforce publication readiness on every published mutation in the database transaction, serializing changes for each chapter. Reject changes that leave no active material/question, unresolved PDFs, invalid answers/objective references, or another failed readiness condition; keep the published content and editor input intact. Whole-chapter/course deletion uses its separate lifecycle. Validate expected revision and threshold atomically at test submission; reject stale submissions without discarding the draft or relabeling old answers as current.

## 4. User experience and implementation sequence

### A. Foundation and authentication

Follow [the task execution guide](tasks/README.md). Task 00 establishes local development; Task 01 design can start independently. Task 00b tracks hosted accounts and email prerequisites for deployment without blocking local features. Create and verify the Android shell in Task 02a immediately after the app shell, then verify native authentication, PDF, and media behavior in Tasks 05, 16, and 17. Task 22 integrates the existing native project rather than introducing Android for the first time.

Set up responsive routing for sign-in, dashboard, course overview, builder, chapter reader, chapter test, flashcards, and download management. Implement email/password registration, email verification, sign-in, password reset, and sign-out. Persist the authenticated session securely through the Supabase client. Expose only the Supabase URL and public client key as frontend configuration.

### B. Course builder

Provide course and chapter creation, editing, reordering, draft preview, publishing, and deletion. Provide a Markdown editor with preview for long-form text; render tables, fenced code, and math, while sanitizing output and rejecting arbitrary author-supplied HTML. Accept only validated HTTPS URLs for external resources. Support YouTube and Vimeo embeds, plus a general external-view fallback. Validate PDF type and the 50 MB size limit before upload. Provide prompt templates for generating a course outline and a complete chapter externally, along with the schema, example JSON, import preview, and validation errors. Do not automatically scrape or copy external articles.

Track PDF upload, replacement, and deletion durably before Storage side effects. Upload each replacement to a unique path, preserve the original until the new link commits, and retry cleanup of unused objects. Mark deletion targets pending and hide them from normal study before removing files; finalize database deletion only after file removal. Persist job state across failures and restarts, reconcile lost responses, and never delete an actively referenced object. Retry unfinished jobs when the same owner next opens the app online; no always-running cleanup service is assumed. Pending cleanup remains visible and cannot be reported as completed.

### C. Reader and progress

Show the course outline, chapter objectives, ordered materials, completion controls, and a clear next action. Render authored Markdown in the application and uploaded PDFs through PDF.js. Embed supported video providers and attempt inline article iframes. Always offer **Open in app view** for external pages: use an isolated InAppBrowser on Android and a new tab on the website, preserving the course location. A remote site may prohibit iframe embedding through its own headers, so the fallback must be visible even when an embed is attempted. [PDF.js](https://mozilla.github.io/pdf.js/getting_started/), [iframe restrictions](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy/frame-ancestors).

Save the most recently opened material. Store Markdown reading position as a heading and scroll offset, and PDF position as a page number. Let the learner explicitly mark each material complete. Derive chapter and course completion from current material progress and passing attempts rather than trusting a separate manually editable completion flag.

### D. Tests and flashcards

Autosave unsubmitted test answers locally so a reload does not discard them. Submit and grade only while online. After submission, show the score, correct answers, explanations, and written-answer rubric. Keep attempts immutable and allow retakes.

Show a chapter's flashcards and a course-wide due-review queue. Use a simple five-box schedule: new cards are due immediately; **Remembered** advances through intervals of 1, 2, 4, 8, and 16 days; **Again** resets the card and returns it to the current session. Flashcard review does not gate chapter completion.

### E. Offline Android reading

Let users explicitly download a chapter's authored text and uploaded PDFs. Store text and cache metadata by user in IndexedDB. Download PDFs into app-private storage with Capacitor File Transfer and Filesystem, using short-lived URLs obtained through the authenticated private Storage API. The bundled Android app shell and downloaded reader content must load without connectivity. Show download state and storage errors. [Capacitor Filesystem](https://capacitorjs.com/docs/apis/filesystem).

Queue offline reading-position and material-completion changes and sync them after reconnection. Completion is monotonic: a stale update cannot undo a completed material. For reading position, use the newest timestamped position for that material. Associate cached data and queued actions with the signed-in user and clear both on sign-out. If a downloaded chapter is revised, keep its current download readable offline and offer a refresh when online. External resources show an offline-unavailable state without losing the learner's place.

Separate online authentication from access to previously authorized downloads. Bind downloads to the last successfully authenticated local owner, and allow that owner's cached reading and queued progress after token expiry and an offline restart without waiting for refresh. This binding grants no backend access or online-only capabilities. Keep auth credentials separate from content caches. On reconnect, validate/refresh the session and match its user ID before syncing. Transient failures preserve offline access; a definitive invalid/revoked session locks it pending online reauthentication by the same owner. Revocation cannot be detected while disconnected. Explicit sign-out locks private routes immediately and durably removes downloads, drafts, queues, owner binding, and local credentials even offline. Interrupted cleanup resumes on restart and blocks new sign-ins until complete. Stage download refreshes so failure preserves the prior readable copy.

### F. Deployment and handoff

Configure Cloudflare Pages for the Vite build and client-side route fallback. Document Supabase project setup, migration application, Auth settings, private bucket policies, environment variables, free-tier limits, and the manual resume procedure for an inactive project. Produce an installable debug APK for the first release and document private release signing for later distribution. Do not commit signing credentials.

## 5. Verification and acceptance

Run focused unit tests for JSON import validation, objective references, question scoring, pass thresholds, completion rules, and flashcard scheduling. Test database and Storage policies with two distinct users to prove one cannot read or modify the other's content. Cover the authoring-to-learning path with browser end-to-end tests.

Manually verify the web interface at desktop and mobile widths and the Android app on an emulator or device. Create a course, import and edit a chapter, upload a PDF, read each material type, encounter a blocked iframe, complete and retake a test, review cards, reload and resume, and check keyboard accessibility and readable text layout. Test failure states for invalid JSON, missing PDF upload, oversized file, unavailable external content, expired authentication, and a paused backend.

For the offline acceptance test, download authored text and a PDF on Android, disable connectivity, let the access token expire, kill/reopen the app, read both, change position and completion, reconnect, and confirm the changes appear on the website after same-owner session validation. Test definitive session rejection and recovery. Sign out offline, interrupt cleanup, restart, and verify private access and new sign-ins remain blocked until all local private data is removed.

Verify a threshold change preserves historical scores/thresholds while requiring a current-revision pass. Test rejection of invalid published edits, including concurrent deletion of the last materials/questions. Test direct attempt UPDATE/DELETE denial alongside authorized parent cascades. Interrupt each PDF lifecycle phase and verify recoverable pending state, safe replacement, and eventual cleanup. Recheck native browser isolation on API 28 and a current Android version.

The implementation is complete when it delivers a deployed website, installable APK, working Supabase migrations and policies, versioned schema and prompt documentation, and reproducible setup and deployment instructions.

## 6. Fixed assumptions and boundaries

- Courses are private to their creator; creator and learner are the same account in version 1.
- No billing account is required. Supabase free-project inactivity pausing is accepted.
- Uploaded files are PDFs no larger than 50 MB. Externally hosted videos are linked or embedded, not uploaded.
- Android offline support covers downloaded authored text and PDFs, plus later sync of reading progress. The website is online-first.
- External article content is embedded only where its host permits; otherwise it opens in the selected fallback view.
- No runtime LLM, collaborative grading, public course discovery, or Play Store release is included.

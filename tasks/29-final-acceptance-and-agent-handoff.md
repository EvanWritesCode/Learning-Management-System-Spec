# Task 29 — Final acceptance and handoff

## Context and dependencies
Tasks 00–28 cover setup, design, all product features, integrated tests, website deployment, and APK creation. This task checks the original requirements as a whole and prepares another maintainer to operate the result.

## Work
Run the acceptance checklist in ../IMPLEMENTATION_PLAN.md against both the deployed website and installed Android APK. Trace each requirement in ../Initial_Requirements.txt to a working screen or documented boundary. Verify sample chapter JSON, prompt kit, design artifacts, migrations, environment example, setup guide, policy tests, web deployment guide, APK install guide, and free-project resume instructions exist and match the delivered code. Record all test commands and outcomes, known limitations, current service URLs, and the location of the APK. Keep credentials and private content out of handoff documents.

## Verification
Reproduce the full scenario with a new test account: create course, import/edit/publish chapter, use Markdown/PDF/video/article materials, pass an in-depth mixed test, review flashcards, resume progress, and read downloaded text/PDF offline on Android before syncing. Check owner isolation with a second account. Resolve any missing requirement or broken acceptance path before declaring completion.

Confirm the regression evidence covers all reviewed risks: API 28 native browser isolation; passPercent revision and historical snapshots; readiness enforcement on published edits; offline restart after token expiry and same-owner reauthentication; interrupted sign-out cleanup blocking account switches; recoverable PDF replacement/deletion; and database-level attempt immutability with authorized parent cascades. Document that remote revocation cannot be detected offline and remote file cleanup resumes when the owner returns online. Confirm Tasks 00b and 02a are included in the handoff and the deployment/native prerequisites are actually verified.

## Done when
Every in-scope requirement has a verified implementation or explicit accepted limitation, and another coding agent can run, test, deploy, and maintain the app from the repository documentation.

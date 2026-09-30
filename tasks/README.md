# Task execution guide

Task IDs remain stable. Two additional tasks separate release prerequisites (00b) and bring native integration forward (02a); Task 22 now verifies the existing Android product.

## Execution order and gates

1. Run [Task 00](00-development-environment-and-accounts.md) for local web/database setup. [Task 01](01-ui-ux-flows-wireframes-and-mockups.md) design can start from the plan without waiting for tools, accounts, or device access.
2. Track [Task 00b](00b-external-accounts-and-release-prerequisites.md) separately. Begin account setup early, but hosted accounts, remote Git authorization, and SMTP gate deployment in Task 27, not local feature work.
3. Build Task 02 after Tasks 00 and 01. Immediately run [Task 02a](02a-early-capacitor-android-shell.md) to establish Android tooling, the native shell, and isolated browser on API 28 and a current version. Task 03 database work depends on local setup, not native setup.
4. Continue Tasks 03–21 using each task's dependencies. Verify Android authentication in Task 05, private PDFs in Task 16, and real media/fallback behavior in Task 17. If device access is pending, continue independent web work but keep native verification and affected task completion explicitly pending.
5. Run Task 22 for integrated Android verification, Tasks 23–24 for offline downloads/sync, and Tasks 25–26 for cross-feature polish and regression.
6. Task 27 requires Task 00b's hosted prerequisites, final Cloudflare authorization, and Task 26's checks. Task 28 produces the installable APK against that backend. Task 29 checks both deliverables and handoff documentation.

## Shared implementation contracts

| Contract | Owning tasks | Required downstream checks |
| --- | --- | --- |
| Android API 28 minimum and isolated browser | 02a, 22 | 05, 16, 17, 26, 28 |
| Immutable attempts and threshold snapshots | 03, 04, 19 | 20, 26; controlled parent cascades in 07/12 |
| Transactional published readiness and revisions, including passPercent | 14 | Wire into 07 and 10–13; verify completion/submission in 18–20 and 26 |
| Recoverable PDF lifecycle and guarded cleanup | 03, 04, 12 | Pending content in 06/15/18; failure recovery in 26 |
| Owner-bound offline access and sign-out cleanup | 05, 23, 24 | Native integration/acceptance in 26, 28, 29 |

Tasks 03–04 establish schema/security contracts that Tasks 12 and 14 extend through tracked migrations. Those later tasks must update earlier authoring paths and re-run policy tests; their completion includes closing direct-API bypasses, not just adding UI validation.

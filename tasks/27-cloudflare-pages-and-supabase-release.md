# Task 27 — Deploy website and configure production services

## Context and dependencies
Task 00b provides hosted accounts, a private Git repository, and the verified email-audience decision; Task 26 provides a working production build and green checks. Local setup in Task 00 does not imply release prerequisites are complete. The chosen stack is Supabase Free plus static Cloudflare Pages; no billing-enabled backend or server functions.

## User actions
Complete Task 00b and authorize Cloudflare Pages to access only this private GitHub repository before deploying. If multi-user sign-up beyond Supabase project-team emails is intended, complete Task 00b's Brevo/domain/SMTP steps and provide confirmation that an outside recipient received test mail. Never send the SMTP password to the coding agent in chat; enter it directly in the Supabase Dashboard. Approve the final public website hostname or custom domain if one is wanted.

## Work
Apply reviewed migrations and Storage policies to the remote Supabase project; do not use destructive remote reset. In Cloudflare Dashboard > Workers & Pages, connect the private repository, choose main as production branch, use root directory /, build command npm run build, output directory dist, and add VITE_SUPABASE_URL plus VITE_SUPABASE_PUBLISHABLE_KEY in Pages environment settings. Set a current Node LTS version for the build if needed. Configure SPA route fallback and verify direct navigation works. Set Supabase Auth Site URL to the final Pages URL and add exact production callback/reset redirect URLs plus localhost:5173 development URLs. Copy the locally tested confirmation and password-recovery email templates into the hosted Auth Email Templates settings so both include the one-time Token code. Test code delivery and entry from the production site and Android app. [Cloudflare build configuration](https://developers.cloudflare.com/pages/configuration/build-configuration/), [Supabase redirect URLs](https://supabase.com/docs/guides/auth/redirect-urls/), [email templates](https://supabase.com/docs/guides/auth/auth-email-templates).

Document free-tier limits, Supabase inactivity pause/resume steps, configuration changes, and rollback by redeploying a previous Pages build. Avoid logging private lesson content or credentials.

## Verification
Check a production deployment from an unauthenticated browser: register/verify or sign in, create/import/study a small course, upload/read/delete a PDF, run a test, and reload a nested route. Confirm another user cannot access private data. Inspect Cloudflare build logs and browser console. If SMTP is not configured, verify and label the site owner-only rather than testing unsupported public sign-up.

## Done when
The website is reachable over HTTPS, production Auth and private data work, deep links reload, and its email audience/limits are accurately documented.

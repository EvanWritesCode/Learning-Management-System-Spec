# Task 22 — Integrated Android verification and polish

## Context and dependencies
Task 02a already created and verified the Android shell and isolated browser. Tasks 05, 16, and 17 verified native authentication, PDFs, and media as they were implemented; Tasks 18–21 complete the online learning features. This task integrates and polishes that existing Android project before offline downloads. Android must be an installable APK, with app UI shared with the website.

## Work
Reuse the tracked Android project and platform adapter from Task 02a; do not regenerate or choose a new application ID. Confirm minSdkVersion 28, compatible pinned plugin versions, and isolated external WebView storage with isIsolated enabled. Integrate the completed online screens, then polish Android hardware back, status/navigation bars, keyboard behavior, external link safety, and return to the same course material. Build and run on the API 28 emulator and a current supported Android version. Keep native configuration reproducible and verify development-only backend/live-reload/cleartext settings are absent from production configuration.

## Verification
Run web build, Capacitor sync, Gradle debug build, and launch on Android. Run authentication, reading/resume, test submission/history, flashcards, external fallback, PDF display, and hardware-back checks. Recheck storage isolation on API 28 and the current version. Confirm min SDK and no service-role key or SMTP secret is packaged. Record results alongside the earlier native feature checks.

## Done when
The shared app launches as a native Android application and the external-page fallback stays inside an isolated app-owned browser view.

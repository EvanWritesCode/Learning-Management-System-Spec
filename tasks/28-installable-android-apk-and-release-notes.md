# Task 28 — Build and validate installable Android APK

## Context and dependencies
Tasks 22–24 provide the native shell and offline features; Task 27 provides the production backend URL. Play Store publication is outside scope. The deliverable is an installable **debug APK**, with release-signing instructions for future distribution.

## Work
Set production Supabase URL/publishable key in the Android build configuration without embedding any server secret. Run the web production build, Capacitor sync, and Gradle debug APK build. Confirm stable app ID/name, app icon/splash from the approved design, minimum SDK 28, enabled InAppBrowser isolation, and Android permissions limited to required capabilities. Remove development-only local backend/live-reload/cleartext settings. Document where the APK is generated, how to install it with adb or device package installer, and how to make a privately signed release build later using a keystore stored outside Git. Do not create or commit a production keystore on the user's behalf.

## Verification
Install the APK on API 28 and a current supported Android version, sign in, study text/PDF/video/article content, complete a test, review cards, and download a chapter. Expire the access token while disconnected, restart offline, read downloaded text/PDF, reconnect and validate the same-owner session before sync. Verify rejected-session recovery, offline sign-out, interrupted cleanup recovery, and a subsequent different-user sign-in. Check hardware back and native browser storage isolation. Inspect the APK/build outputs for service-role, database, or SMTP secrets.

## Done when
A fresh device can install and use the APK against the production backend, including the agreed offline reading workflow, and the build is reproducible from documented commands.

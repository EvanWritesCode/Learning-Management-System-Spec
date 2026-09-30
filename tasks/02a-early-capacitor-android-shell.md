# Task 02a — Early Android shell and platform check

## Context and dependencies
Run immediately after Tasks 00 and 02 provide a buildable responsive app shell. Do not wait for Tasks 16–17 or hosted accounts. This establishes the Android project once; Task 22 later verifies the integrated product. Web-only work may continue if device access is pending, but record the native check as incomplete rather than claiming Android support is verified.

## Work
Inspect existing tools and install missing Android Studio, SDK/platform tools, emulator, and bundled JDK from official sources. Install SDK components required by the pinned Capacitor version. Use an emulator or a device with USB debugging; document any required Windows driver. Create an API 28 emulator for minimum-version verification and also test a current supported Android version.

Initialize Capacitor at the repository root with a stable application ID/name, webDir set to dist, and a tracked Android project. Set minSdkVersion to 28 (Android 9), while using the compile/target SDK required by the pinned toolchain. Pin an InAppBrowser version that supports isolated openInWebView, keep isIsolated enabled, and verify the merged manifest and actual browser behavior. API 26–27 are outside this release because their WebViews cannot provide the required storage isolation. [Capacitor isolation documentation](https://capacitorjs.com/docs/apis/inappbrowser#localstorage-isolation).

Create the shared platform adapter with web new-tab and Android isolated in-app browser implementations, a visible close action, safe HTTPS navigation, and hardware-back handling. Provide a minimal shell-level launch action or development fixture to test it before materials exist. Keep generated native configuration reproducible and exclude build outputs. Document local Supabase access from the emulator/device using development-only addresses/settings; production builds must not retain local endpoints, live-reload server URLs, or debug-only cleartext exceptions. Do not expose the local backend publicly.

## Verification
Build the web bundle, run Capacitor sync, build/install the debug APK, and navigate shell routes on API 28 and a current Android version. Open/close an external page and test hardware back. Use controlled test content to verify cookies/local storage do not cross between the main WebView and external WebView; absence of the app bridge alone is not proof of storage isolation. Verify plugin isolation is enabled and no server secrets are packaged. Record exact versions, commands, and device/API results.

## Done when
The shared shell runs on Android and the native browser adapter is verified on the minimum supported API. Task 05 adds the native authentication check, Task 16 adds private-PDF verification, Task 17 adds real media/fallback verification, and Task 22 integrates the completed features without regenerating the project.

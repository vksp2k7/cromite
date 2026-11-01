# Google sign-in and Gemini/Glic port

This branch starts an opt-in port of the upstream Chromium Android implementations into Cromite. It is based on the user-provided authorized Chrome APK for static comparison only; no account credentials are used and the APK is never executed.

## Reference artifact

- File: `com.android.chrome_156.0.8078.79-807807904_minAPI29(arm64-v8a)(nodpi)_apkmirror.com.apk`
- SHA-256: `eeef4842073b5f99b5429ede221b4db7fe2dbe17a5a513c738dde38bd1fb8b26`
- REA evidence: `ev_397c5a7274847ef921a8ad7164a6e1e8d802d69266d7043ce624ad3758ab153d`
- Static signer declaration: Google Inc. / Android, v2 and v3; REA reports self-consistent APK content digests. REA does not independently verify certificate authenticity or provenance.

## Initial source change

`build/cromite_patches_list.txt` no longer applies these Cromite disable patches:

- `kill-Auth.patch`
- `Remove-binary-blob-integrations.patch`
- `Disable-integration-with-Gemini.patch`

This restores the corresponding Chromium source paths, including Android account-manager integration and Chromium's Glic/Gemini UI/service code. Removing `Remove-binary-blob-integrations.patch` is intentionally broad: it also restores other Google Play Services-backed integrations, not only sign-in. This must be reviewed before distribution because it changes Cromite's privacy and dependency posture.

## Current verification status

- The change is on local branch `port/google-signin-gemini-android`.
- REA APK signature inspection completed successfully.
- REA Android class/method inspection is pending because the exact `jadx-headless-mcp` 0.7.1 JAR required by the installed REA release is not available in the configured environment.
- A complete Chromium checkout/toolchain is not present in this repository checkout; the repository contains Cromite's patch stack and build configuration. The arm64 release build therefore has not yet run.
- The requested build target remains `is_debug=false`, `is_official_build=true`, `target_cpu="arm64"`, with a local sample release keystore rather than a production signing key.

## Security and distribution notes

Do not place Google OAuth secrets, refresh tokens, or production signing keys in this repository. A production build must use a user-owned OAuth configuration and a separately managed signing key. The local sample key is suitable only for installation/testing and must not be presented as an official Cromite or Chrome release.

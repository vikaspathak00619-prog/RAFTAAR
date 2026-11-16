# RAFTAAR V6 In-App Update Configuration

This repository is reserved for the RAFTAAR Android app's public update metadata.
It is intentionally separate from the payment backend and the RAFTAAR V5 Android code.

## Files

- `version.json`: public update manifest read by the Android app.
- APK binaries should be published as GitHub Release assets (not committed into this repository).

## Current safe state

The manifest is deliberately disabled (`enabled: false`) until the V6 Android updater is implemented, tested, and the first signed V6 APK is ready. No existing user should be directed to an empty or unverified download.

## Manifest fields

| Field | Meaning |
|---|---|
| enabled | Enables update prompts only after a real APK is published |
| app | Product identifier |
| channel | Release channel, currently stable |
| latestVersionCode | Android versionCode of the latest APK |
| latestVersionName | Human-readable version |
| minimumSupportedVersionCode | Versions below this value must update |
| mandatory | Whether the update is mandatory |
| apkUrl | HTTPS URL of the APK release asset |
| sha256 | SHA-256 hex digest of the exact APK |
| releaseNotes | Short update description |

## Release checklist

1. Build the V6 APK with the same applicationId and signing key as the previously installed V6 app. Keep the V5 applicationId separate so V5 remains an independent app.
2. Increase Android `versionCode` for every release.
3. Publish the APK as a GitHub Release asset.
4. Calculate SHA-256 from the exact APK file.
5. Update `version.json` with the actual versionCode, versionName, HTTPS asset URL and SHA-256.
6. Test on dedicated devices: version check, optional/mandatory prompt, download, checksum verification, Android install confirmation, cancel/retry, and Remind Me Later.
7. Only then set `enabled` to true.

## Safety

- Do not store user data, credentials, payment details, API keys, or secrets here.
- Do not modify or deploy `Raftaar-Payment-Backend` for this updater.
- Do not change the live V5 APK or its package/signing identity.
- Android requires user confirmation for sideload installation unless the device is managed with special privileges.
- A GitHub manifest alone cannot update an installed app; the Android V6 client must implement the updater.

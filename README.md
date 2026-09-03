# Full Frills Balance Configs

This repository stores the remote configuration used to control Full Frills Balance app updates.

The mobile app fetches a static JSON manifest from this repository through GitHub Raw. The manifest is consumed by the app's update gate and determines whether an installed iOS or Android build must be updated before the app can be used.

The app should be configured with:

```text
EXPO_PUBLIC_VERSION_POLICY_URL=https://raw.githubusercontent.com/SahilSoniOrg/full-frills-balance-configs/main/version-policy.json
```

## Policy manifest

The live file should be named `version-policy.json` and contain policies for all supported platforms:

```json
{
  "ios": {
    "minimumBuild": 42,
    "latestBuild": 45,
    "storeUrl": "https://apps.apple.com/app/id0000000000",
    "message": "Please update Full Frills Balance to continue.",
    "availableMessage": "A newer version of Full Frills Balance is available.",
    "enabled": true
  },
  "android": {
    "minimumBuild": 38,
    "latestBuild": 41,
    "storeUrl": "https://play.google.com/store/apps/details?id=in.sahilsoni.fullfrillsbalance",
    "message": "Please update Full Frills Balance to continue.",
    "availableMessage": "A newer version of Full Frills Balance is available.",
    "enabled": true
  },
  "web": {
    "minimumBuild": 1,
    "storeUrl": "https://example.com",
    "message": "Please refresh Full Frills Balance.",
    "enabled": false
  }
}
```

`version-policy.example.json` contains the same contract without being used by the app.

## Field behavior

- `minimumBuild` is required for every platform and is compared with the native build number on iOS and Android.
- `storeUrl` is required and is opened by the **Update now** button on mobile.
- `message` is optional; the app uses its default message when omitted.
- `availableMessage` is optional; it is shown in the non-blocking update notice when `latestBuild` is newer than the installed build.
- `enabled` is optional and defaults to enabled. Set it to `false` to bypass enforcement for that platform.
- `latestBuild` is informational for now and is not used to decide whether access is blocked.
- Web is included in the manifest for future hard-refresh handling, but the current app does not enforce it.

## Changing the policy

1. Edit `version-policy.json`.
2. Validate the JSON and review the build numbers and store URLs.
3. Commit the change with a clear message.
4. Monitor the app after publishing; GitHub Raw may cache the file briefly.

Keep this repository public. The app cannot safely include credentials to read a private GitHub repository.

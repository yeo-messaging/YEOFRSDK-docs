# Upgrade and re-enrol

If your app is on an older version of the SDK, moving to the current one needs a one-time re-enrolment. This guide shows how to plan it so that your users hardly notice.

## Do users need to enrol again?

| Moving from | To | Re-enrol? |
|---|---|---|
| Android before 0.2.28 | Android 0.2.29 | **Yes, once** |
| iOS before 0.11.0 | iOS 0.11.0 or 0.11.2 | **Yes, once** |
| React Native 0.4.x or earlier | React Native 0.5.2 | **Yes, once** |
| Flutter 0.2.x or earlier | Flutter 0.5.1 | **Yes, once** |
| Android 0.2.28 | Android 0.2.29 | No |
| iOS 0.11.0 | iOS 0.11.2 | No |
| React Native 0.5.0 or 0.5.1 | React Native 0.5.2 | No |
| Flutter 0.5.0 | Flutter 0.5.1 | No |

The current versions use a new on-device recognition model, and the SDK refuses a gallery made by an older model. This is safe: nothing crashes, and the app simply has no enrolment until the user enrols again.

## What changes for your developers

- **Liveness rule.** Use `isLive` or `status`, or `faceState == Detected && isConfident()` in native Android. Remove any comparison with `0.71`. In native Android, recompile every module that reads `ANTI_SPOOF_THRESHOLD`.
- **Android repository.** The Android SDK moved to a new private repository with a new login from YEO. Update your `maven { }` block.
- **Android build.** Your app needs `compileSdk` 36 and Android Gradle Plugin 8.9.1 or later. Google Play services is needed on the device.
- **iOS.** iOS 17.0 or later is required. A few iOS calls became `async`, and the model-selection options are gone.
- **React Native on iOS.** Camera enrolment is now a single front capture, so adjust any screen that prompted a walk on iOS.
- **Credential.** A valid credential from YEO is required on both platforms.

## How to plan the re-enrolment

1. **Before release,** decide what the app will say, for example: "We've upgraded face sign-in. Please set it up again. It takes about a minute." Decide which fallback sign-in users have until they do.
2. **Detect the refused gallery** at start-up, right after starting the SDK:

    | Platform | The sign |
    |---|---|
    | Android | `loadTracker(...)` returns a negative code. |
    | iOS with `FaceTrustService` | `enrolledName` has no value after `start()`. |
    | React Native | `loadTrackerFromDisk()` resolves `false`. |
    | Flutter | `loadTracker` returns a negative code. |

3. **Clean up.** Delete the old stored gallery so that old biometric data does not linger. A refused file stays on disk until your app saves over it or deletes it. Delete galleries from older versions held on a server once the user has re-enrolled.
4. **Prompt the user** to enrol with the [guided capture](enrol.md), in their usual place and light.
5. **Save** the new gallery straight away, and again after sign-in sessions.
6. **Roll out in stages** if you can, and watch support contacts for "not recognised" in the first week. The usual causes are backlight and quick single-frame enrolments.

## What to tell your users

> After the update, face sign-in needs to be set up once more. Open the app, choose *Set up face sign-in*, face a light, and follow the prompts.

## Staying on the same version line

Galleries carry over within the current versions. Check each release's notes for anything else you need to change. See [Published versions](../reference/versions.md).

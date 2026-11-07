# Data and privacy

Written for your security, privacy and compliance teams. It describes the published versions listed on [Published versions](reference/versions.md).

## In short

- Face processing happens on the phone. The SDK makes no network connection of its own.
- The SDK saves numeric face samples, never photos or video.
- Your app controls the saved enrolment, including where it is kept and how it is encrypted. The SDK does not encrypt it by default.
- On Android, Google components used for face detection have their own network activity. [See below](#what-leaves-the-device).
- Device trust and proof of a result to your backend are your app's responsibility. [See below](#device-and-app-security).

## What the SDK stores

| Stored | Details |
|---|---|
| Enrolled names | Whatever your app passes as the name. We recommend an opaque ID. |
| Face samples | Up to 25 from enrolment and up to 25 recent ones per person. Each is a set of numbers computed from one camera frame. |
| Timestamps and version details | When the person was enrolled, the format version, and the recognition model's version. |

**The SDK does not store** photos, video or camera frames. Frames are processed in memory and are not saved.

!!! note "`keepFaceImagesInTracker` on Android"
    The Android API reference lists this option and describes storing images. That wording comes from an earlier engine. In Android 0.2.29 the option has no effect, and no image is stored with either value.

For where the enrolment goes by default on each platform, see [Store the gallery](guides/store-the-gallery.md#what-happens-by-default).

## What leaves the device

### From the SDK

Nothing. The SDK sends no faces, samples, names, results or usage data to YEO or anyone else. It contains no analytics or crash-reporting product, and the SDK itself declares no internet permission. Starting the SDK with your credential does not contact YEO either.

Your app can of course send data itself. For example, it may upload an encrypted gallery if you choose to keep one on your server.

### On iOS

Nothing beyond the SDK statement above. Face detection uses Apple's Vision framework, which is part of iOS and runs on the device. The SDK uses no Google code on iOS.

### On Android

Face detection uses Google's ML Kit, through Google Play services. This is separate from the SDK:

- **Model download.** The face detection model is not inside your app. Google Play services downloads and updates it, starting on first use.
- **Usage metrics.** ML Kit sends Google its own diagnostics and usage metrics, such as device and app information, performance and error codes. Google states that images and detection results stay on the device. The SDK does not turn this logging off.
- **Permissions.** ML Kit adds `INTERNET` and `ACCESS_NETWORK_STATE` to your app's merged manifest.

Google's terms make the app developer responsible for telling users about this and for the Google Play Data safety form. Read [Google's ML Kit data disclosure](https://developers.google.com/ml-kit/android-data-disclosure) and the [ML Kit terms](https://developers.google.com/ml-kit/terms).

### At build time

These happen on your build machines, not on users' phones: the iOS Swift package comes from GitHub, and the Android package comes from YEO's private Maven repository.

## Models

YEO's recognition and liveness models are bundled inside the SDK. They change only when you ship an SDK update in your own app, and the SDK does not download them.

## Encryption and who is responsible

| Topic | The SDK | Your app |
|---|---|---|
| Processing of faces | Happens on the phone | |
| Encryption of the saved enrolment | Not encrypted by default. iOS offers a hook for your own cipher. | Encrypts it, with keys in the Keychain or Keystore |
| Location of the saved enrolment | Private storage, where a built-in store is used | Chooses it, or takes over storage |
| Backup and device transfer | Excluded by Capacitor and Cordova only | Excludes it everywhere else |
| Deletion | Provides the calls | Calls them, and removes its own copies |
| Network transfer | Does none | Protects anything it sends |
| Privacy notices and consent | | Yours to provide |
| Retention | Keeps samples until cleared | Sets the policy |

See [Store the gallery](guides/store-the-gallery.md) for how to do each of these.

## Device and app security

The SDK's liveness check looks at what is shown to the camera. The SDK does not assess the device or app it runs on, so these are for your app to handle.

| Threat | In the SDK? | What we suggest |
|---|---|---|
| Rooted or jailbroken device | Not detected | Google Play Integrity or Apple App Attest in your app, and your own policy to block or step up |
| Emulator | Not detected | The same device checks |
| Hooking or tampering tools | Not detected | Runtime protection or app shielding, if your policy requires it |
| Screen sharing or capture | Not detected or blocked | `FLAG_SECURE` on the Android face screen, and screen-capture checks on iOS |
| Virtual camera | No check on where frames come from | Rely on the device checks above. On Android, make sure your app opens the device's front camera. |

### Results are not signed

The SDK returns results to your app as ordinary data structures. It does not sign them, and it does not use a hardware-backed key to do so. To rely on a result on your server, use your own protections, such as App Attest or Play Integrity and your existing secure channel.

!!! tip "Think of the result as local"
    A result proves something to your app on that device. Design your backend so that a decision is bound to your own session and device checks.

## Biometric data

The gallery is biometric data. Check your obligations under the rules that apply to you, and treat it accordingly in your records, notices and retention policy. These pages describe what the software does. They are not legal advice.

## Questions from your compliance team

Common questions are answered in the [FAQ](faq.md#security-and-compliance). For anything else, [contact YEO](support.md).

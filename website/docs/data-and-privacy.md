# Data and privacy

Written for your security, privacy and compliance teams. It describes the published versions listed on [Published versions](reference/versions.md).

## In short

- Face processing happens on the phone. The SDK makes no network connection of its own.
- The SDK saves numeric face samples, never photos or video.
- Your app holds the saved enrolment and encrypts it with keys you control.
- On Android, face detection uses Google's ML Kit. [See below](#what-leaves-the-device).
- Your app applies your device trust and backend checks. [See below](#device-and-app-security).

## What the SDK stores

| Stored | Details |
|---|---|
| Enrolled names | Whatever your app passes as the name. We recommend an opaque ID. |
| Face samples | Up to 25 from enrolment and up to 25 recent ones per person. Each is a set of numbers computed from one camera frame. |
| Timestamps and version details | When the person was enrolled, the format version, and the recognition model's version. |

**The SDK does not store** photos, video or camera frames. Frames are processed in memory and are not saved.

For how your app holds and protects the enrolment, see [Store the gallery](guides/store-the-gallery.md).

## What leaves the device

### From the SDK

Nothing. The SDK sends no faces, samples, names, results or usage data to YEO or anyone else. It contains no analytics or crash-reporting product, and the SDK itself declares no internet permission. Starting the SDK with your credential does not contact YEO either.

Anything your app sends is under your control, such as an encrypted gallery if you keep one on your server.

### On iOS

Nothing beyond the SDK statement above. Face detection uses Apple's Vision framework, which is part of iOS and runs on the device. The SDK uses no Google code on iOS.

### On Android

Face detection uses Google's ML Kit, through Google Play services, which also downloads and updates the face detection model. Google's own terms cover ML Kit. Read [Google's ML Kit data disclosure](https://developers.google.com/ml-kit/android-data-disclosure) and the [ML Kit terms](https://developers.google.com/ml-kit/terms) when you prepare your privacy notice and your Google Play Data safety form.

### At build time

These happen on your build machines, not on users' phones: the iOS Swift package comes from GitHub, and the Android package comes from YEO's private Maven repository.

## Models

YEO's recognition and liveness models are bundled inside the SDK. They change only when you ship an SDK update in your own app, and the SDK does not download them.

## Encryption and who is responsible

| Topic | The SDK | Your app |
|---|---|---|
| Processing of faces | Happens on the phone | |
| Encryption of the saved enrolment | Gives your app the data. iOS offers a hook for your own cipher. | Encrypts it, with keys in the Keychain or Keystore |
| Location of the saved enrolment | Provides calls to read and restore it | Chooses where it lives |
| Backup and device transfer | | Excludes it |
| Deletion | Provides the calls | Calls them, and removes its own copies |
| Network transfer | Does none | Protects anything it sends |
| Privacy notices and consent | | Yours to provide |
| Retention | Keeps samples until cleared | Sets the policy |

See [Store the gallery](guides/store-the-gallery.md) for how to do each of these.

## Device and app security

Your app is the right place for these controls, because they follow your own policy.

| Area | What we suggest |
|---|---|
| Rooted or jailbroken devices | Google Play Integrity or Apple App Attest in your app, and your own policy to block or step up |
| Emulators | The same device checks |
| Hooking or tampering tools | Runtime protection or app shielding, if your policy requires it |
| Screen sharing or capture | `FLAG_SECURE` on the Android face screen, and screen-capture checks on iOS |
| Camera source | The device checks above. On Android, have your app open the device's front camera. |

### Relying on a result on your server

The SDK returns results to your app. To rely on one on your server, use App Attest or Play Integrity and your existing secure channel, and bind the decision to your own session and device checks.

## Biometric data

The gallery is biometric data. Check your obligations under the rules that apply to you, and treat it accordingly in your records, notices and retention policy. These pages describe what the software does. They are not legal advice.

## Questions from your compliance team

Common questions are answered in the [FAQ](faq.md#security-and-compliance). For anything else, [contact YEO](support.md).

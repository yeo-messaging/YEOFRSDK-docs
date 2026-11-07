# Get started on iOS

Version 0.11.2. `FaceTrustService` is the quickest integration: it runs the camera, makes the trust decision and saves the enrolment.

## 1. Add the package

In Xcode choose **File ▸ Add Package Dependencies…** and enter the HTTPS address:

```text
https://github.com/yeo-messaging/YEOFR-SPM
```

Choose exact version `0.11.2`. Your deployment target must be iOS 17.0 or later.

Add the camera usage text to `Info.plist`, worded for your users:

```xml
<key>NSCameraUsageDescription</key>
<string>Used to verify your identity with face recognition.</string>
```

## 2. Start the service

```swift
import YEOFR

let service = FaceTrustService(pilotUnlockCode: credentialFromYeo)
// Attach service.previewSession to an AVCaptureVideoPreviewLayer.
try await service.start()
```

## 3. Enrol the user

```swift
for await step in service.enroll(name: "user-id") {
    // step.phase: .capturing (show step.prompt), then .completed or .failed
}
```

One call captures one sample from the first frame that shows exactly one face. Call it again under other angles and lighting to add samples, up to 25.

Only start enrolment after you have confirmed who the user is by another means, and require one `.trusted` result before you turn on face sign-in. See [Enrol a user](../guides/enrol.md#protect-the-enrolment-step).

## 4. Recognise the user

```swift
for await update in service.updates() {
    // update.status: .noFace, .badFraming, .notTrusted or .trusted
    // update.matchedName, update.livenessGenuine, update.depthIsThreeD
}
```

Accept when `update.status == .trusted` **and** `update.matchedName` is the person you expect. With no enrolment in place, `.trusted` can be reported for any live face, so always check the name. See [Recognise a user](../guides/recognise.md).

## 5. Decide where the enrolment is saved

`FaceTrustService` saves the enrolment for you and restores it at the next launch. By default the file is unencrypted, sits in the app's Documents folder and is included in device backups. For production, move it and encrypt it. See [Store the gallery](../guides/store-the-gallery.md).

## Keep going

- The simulator builds and runs your app, but every frame reports no face.
- [API reference for iOS 0.11.2](https://yeo-messaging.github.io/YEOFRSDK-docs/0.11.2/documentation/yeofr/)
- [Go-live checklist](../guides/go-live.md)

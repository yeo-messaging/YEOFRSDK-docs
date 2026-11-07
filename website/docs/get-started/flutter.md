# Get started with Flutter

Version 0.5.1, which uses the Android SDK 0.2.29 and the iOS SDK 0.11.0. Your app runs the camera and the capture loop, and the plugin gives you a result for each frame.

The plugin is installed from YEO's private Git repository, not from pub.dev. Ask YEO for access. The [plugin README](https://github.com/YEOMessaging/yeofr-flutter/tree/v0.5.1#readme) has every setup step in full, including the Android repository block and the iOS Podfile hook.

## 1. Install

```yaml
dependencies:
  yeofr_flutter:
    git:
      url: https://github.com/YEOMessaging/yeofr-flutter
      ref: v0.5.1
```

**Android:**

- Add YEO's Maven repository, using the address and login from YEO.
- Add `implementation 'com.yeomessaging:yeofr-android:0.2.29'` to your app. The version must match exactly.
- Build with `compileSdk` 36 and Android Gradle Plugin 8.9.1 or later.
- Add the camera permission.

**iOS:**

- Add `NSCameraUsageDescription` to `Info.plist`.
- Add the download hook from the README to your `Podfile`, then run `pod install`. The minimum is iOS 17.0, on a real device only.

## 2. Start the plugin

```dart
await YeoFR.init(pilotUnlockCode: credentialFromYeo);
await YeoFR.createTracker();

final saved = await loadTrackerFromStorage();   // your own storage
if (saved != null) await YeoFR.loadTracker(saved);
```

## 3. Read results from the camera

Use the `camera` package. iOS needs `bgra8888` and Android uses `yuv420`.

```dart
await controller.startImageStream((CameraImage image) async {
  final result = await YeoFR.detectFacesFromCameraImage(
    image: image,
    camera: camera,
  );
  // result.isLive, result.framing, result.faceIDs
});
```

## 4. Enrol the user

Each `enroll` call adds one sample, so one call gives you one sample. For a production enrolment, call it on several live, well-framed frames during a short guided capture, then save. See [Enrol a user](../guides/enrol.md).

```dart
if (result.faceState == FaceState.detected && result.isLive) {
  final faceId = result.faceIDs.firstOrNull?.index;
  if (faceId != null) await YeoFR.enroll(faceId, 'user-id');   // 0 means success
}

// When the capture is finished:
final bytes = await YeoFR.saveTracker();
if (bytes != null) await saveTrackerToStorage(bytes);   // your own storage
```

## 5. Recognise the user

```dart
final recognised = result.isLive && result.faceIDs.firstOrNull?.id == expectedUser;
```

Accept once this has held for about a second. Never compare `confidence` with a number. See [Recognise a user](../guides/recognise.md).

!!! note "No depth check in Flutter"
    The plugin passes your camera frames to the SDK, so the iPhone's TrueDepth sensor is not part of the decision.

## Keep going

- Exclude your saved gallery from Android backup and device transfer. See [Store the gallery](../guides/store-the-gallery.md).
- [Go-live checklist](../guides/go-live.md)

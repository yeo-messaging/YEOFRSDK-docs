# Get started with React Native

Version 0.5.2, which uses the Android SDK 0.2.29 and the iOS SDK 0.11.0. The `<YeofrCamera>` view runs the camera and sends your JavaScript one result per analysed frame.

## 1. Install

```bash
npm i @yeomessagingcom/react-native-yeofr
npx yeofr-setup
npx yeofr-setup doctor
```

The setup command adds the iOS Swift package and the Android repository block, then runs `pod install`. `doctor` checks each step.

Then set these up by hand:

- **Android repository access.** Put the address and login from YEO in `~/.gradle/gradle.properties`, or in environment variables on your build server.
- **Android camera permission.** The package declares no permissions. Add `android.permission.CAMERA` to your manifest and request it at runtime before you show the camera.
- **iOS deployment target.** Set `platform :ios` to `'17.0'` in your `Podfile`, and add `NSCameraUsageDescription` to `Info.plist`.

!!! note "Camera view on iOS"
    The iOS camera view needs React Native's old architecture, so set `RCT_NEW_ARCH_ENABLED=0` if you use it. The other calls work with the New Architecture on, and so does the Android camera view.

The [React Native guide](https://yeo-messaging.github.io/YEOFRSDK-docs/rn/) covers manual setup, Xcode 26 and every option.

## 2. Activate

```ts
import {activate, loadTrackerFromDisk} from '@yeomessagingcom/react-native-yeofr';

await activate({pilotUnlockCode: credentialFromYeo});
const enrolled = await loadTrackerFromDisk();   // false if there is nothing to restore
```

On Android a refused credential rejects the call. On iOS it closes the app. See [Before you start](before-you-start.md#get-your-credential).

## 3. Enrol the user

Set `enrolName` on the camera and wait for the `registered` event. Then save.

```tsx
<YeofrCamera
  style={StyleSheet.absoluteFill}
  enrolName={enrolling ? 'user-id' : undefined}
  enrolPoses={['front', 'yawLeft', 'yawRight', 'pitchUp', 'pitchDown']}
  onEvent={(e) => handle(e.nativeEvent)}
/>
```

```ts
if (event.type === 'registered') await saveTrackerToDisk();
```

On Android, `enrolPoses` makes the capture a paced walk that takes at least five seconds. On iOS the capture is a single front capture, whatever the poses list says. See [Enrol a user](../guides/enrol.md).

## 4. Recognise the user

```ts
function handle(event: CameraEvent) {
  if (event.type !== 'result') return;
  const accepted = event.status === 'trusted' && event.matchedName === expectedUser;
}
```

Accept once this has held for about a second. Never compare `confidence` with a number. See [Recognise a user](../guides/recognise.md).

## Keep going

- [React Native guide](https://yeo-messaging.github.io/YEOFRSDK-docs/rn/)
- [Store the gallery](../guides/store-the-gallery.md)
- [Go-live checklist](../guides/go-live.md)

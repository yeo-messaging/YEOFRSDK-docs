# Get started with Capacitor

Version 0.5.0, which uses the Android SDK 0.2.29 and the iOS SDK 0.11.2. It works with any web framework in a Capacitor 7 or 8 shell, with or without Ionic. The native layer owns the camera and draws the preview behind your page. Your page gets small JSON events, and camera frames never reach JavaScript.

## 1. Install

```bash
npm install @yeomessagingcom/capacitor-yeofr
npx cap sync
```

**iOS:**

- Set the deployment target of the App target to 17.0, then run `npx cap sync` again.
- Add `NSCameraUsageDescription` to `Info.plist`.
- You need Xcode 26 or later.
- On Capacitor 8, use Swift Package Manager. On Capacitor 7, use CocoaPods and add the YEOFR Swift package to the App target at exact version `0.11.2`.

**Android:**

- Add the address and login from YEO to `~/.gradle/gradle.properties`, or to environment variables on your build server.
- Add the plugin's repository and core module settings, as shown in the [Capacitor guide](https://yeo-messaging.github.io/YEOFRSDK-docs/capacitor/0.5.0/).
- Build with `compileSdk` 36 and Android Gradle Plugin 8.9.1 or later.

**Your page:** make it transparent where the camera should show. The guide shows how. Lock the page or app to portrait, because the preview does not follow device rotation.

## 2. Activate and listen

```ts
import { Yeofr } from '@yeomessagingcom/capacitor-yeofr';

await Yeofr.activate({ pilotCode: credentialFromYeo });   // once per process
const results = await Yeofr.addListener('result', (r) => {
  // r.status: 'noFace' | 'badFraming' | 'notTrusted' | 'trusted'
  // r.matchedName, r.isLive, r.framing
});
await Yeofr.start();
```

On Android a refused credential rejects with `activationFailed`. On iOS it closes the app. See [Before you start](before-you-start.md#get-your-credential).

## 3. Enrol the user

```ts
await Yeofr.enrol({
  name: customerId,
  poses: ['front', 'yawLeft', 'yawRight', 'pitchUp', 'pitchDown'],
});
// 'enrolProgress' events arrive, then 'registered'
```

The plugin keeps one identity at a time. A new capture replaces the old one, and a capture that is cut short puts the previous enrolment back. See [Enrol a user](../guides/enrol.md).

## 4. Recognise the user

Act only on `status === 'trusted'`, and check that `matchedName` is the person you expect. See [Recognise a user](../guides/recognise.md).

```ts
await Yeofr.stop();
await results.remove();
```

## 5. Decide where the template is kept

By default the plugin keeps it in private app storage, out of device backups, and does not encrypt it. We recommend storing it encrypted: read it with `getTrackerData()` and put it back with `loadTracker()`. See [Store the gallery](../guides/store-the-gallery.md).

## Keep going

- [Capacitor guide 0.5.0](https://yeo-messaging.github.io/YEOFRSDK-docs/capacitor/0.5.0/)
- [Go-live checklist](../guides/go-live.md)

# Get started with Cordova

Version 0.5.0, which uses the Android SDK 0.2.29 and the iOS SDK 0.11.2. It works with any web framework on Cordova, including Ionic Angular apps. The calls, events and statuses are the same as in the Capacitor plugin.

## Web frameworks

The plugin works with Angular, React, Vue, Svelte and plain JavaScript, with or without Ionic. The calls on this page are the same in all of them.

The [Cordova guide](https://yeo-messaging.github.io/YEOFRSDK-docs/cordova/) has an Ionic Angular quick start. In other frameworks, start the camera when the face page opens and stop it when the page closes.

## 1. Install

```bash
cordova plugin add @yeomessagingcom/cordova-plugin-yeofr
```

**Android (`cordova-android` 14 recommended, 13 supported):**

- Add the address and login from YEO to `~/.gradle/gradle.properties`, or to environment variables on your build server.
- Set the required Android build preferences in `config.xml`, and build with JDK 17 or later.

**iOS (`cordova-ios` 8 recommended, 7 supported):**

- Set `deployment-target` to 17.0 in `config.xml`.
- The plugin adds a default camera usage text. You can reword it when you add the plugin, as the guide shows.
- You need Xcode 26 or later.

**Your page:** make it transparent where the camera should show, and lock the app to portrait.

The [Cordova guide](https://yeo-messaging.github.io/YEOFRSDK-docs/cordova/) walks through all of this, with a full Ionic Angular example.

## 2. Activate and listen

The plugin is available as `cordova.plugins.Yeofr` after `deviceready`.

```ts
const Yeofr = cordova.plugins.Yeofr;

await Yeofr.activate({ pilotCode: credentialFromYeo });
const results = await Yeofr.addListener('result', (r) => {
  // r.status: 'noFace' | 'badFraming' | 'notTrusted' | 'trusted'
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

See [Enrol a user](../guides/enrol.md).

## 4. Recognise the user

Act only on `status === 'trusted'`, and check that `matchedName` is the person you expect. See [Recognise a user](../guides/recognise.md).

## 5. Decide where the template is kept

By default the plugin keeps it in private app storage, out of device backups, and does not encrypt it. We recommend storing it encrypted: read it with `getTrackerData()` and put it back with `loadTracker()`. See [Store the gallery](../guides/store-the-gallery.md).

## Keep going

- [Cordova guide](https://yeo-messaging.github.io/YEOFRSDK-docs/cordova/)
- [Go-live checklist](../guides/go-live.md)

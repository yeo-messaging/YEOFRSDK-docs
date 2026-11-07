# Frequently asked questions

## Continuous verification

### What does "continuous" mean?

The SDK reports on every analysed camera frame, so your app can confirm that the right person is present throughout a period you choose, not only at login. See [Verify throughout a session](guides/continuous.md).

### Do users get prompted during a session?

The SDK does not prompt anyone. The check is passive, and your app decides whether and when to show a message.

### What happens if the check fails during a session?

Your app decides. It might ask the user to verify again, hide sensitive content, block a risky action, flag the session or end it. The SDK provides the signal, and your app controls the response.

### Does it replace our login or identity checks?

No. It runs alongside your existing login and onboarding, without changes to that infrastructure.

### Do we need servers to run it?

No. Processing happens on each phone, so there is no YEO server for you to run or call.

### Does it work in a web browser?

No. The SDK runs in iOS and Android apps. Web apps can use it through a Capacitor or Cordova shell, but not in a browser on its own.

### Can it match our brand?

Yes. The SDK has no YEO-branded screens in your app. Your app owns the layout, wording and prompts.

## Enrolment

### How long does enrolment take?

It depends on your app's capture screen. A quick capture takes about a second. The guided capture we recommend for production, with a few small head turns, takes from about 5 seconds to around a minute.

### How many photos are stored?

None. The SDK stores numeric face samples, not images: up to 25 from enrolment and up to 25 recent ones per person.

### Does it work with glasses?

Yes, with clear everyday glasses. If the user sometimes wears glasses and sometimes does not, capture part of the enrolment each way. Sunglasses should be removed.

### Does it work with a face mask, hat or hood?

Please remove them for enrolment and sign-in. Covering the nose and mouth, or shadowing the eyes, prevents reliable recognition.

### What about beards, haircuts, make-up and ageing?

Gradual changes may be absorbed, because successful sign-ins can refresh a set of recent samples where your app saves them. See [Getting better over time](guides/how-it-works.md#getting-better-over-time). After a large sudden change, such as shaving a full beard, re-enrol if recognition becomes unreliable.

### Can several people be enrolled in one app?

We recommend one person per app installation. If you need several, contact YEO first so that we can advise on identity handling for your platform.

### What does the user see if enrolment fails?

Your app decides the wording. The SDK reports when no face, several faces or a badly placed face is in view. A good capture screen shows the framing hint and a progress indicator, and shows tips after about a minute without progress.

### Does the enrolment capture check liveness?

It depends on the platform. On Android, the React Native, Capacitor and Cordova captures only count live frames, and native Android and Flutter samples check liveness before enrolling. On iPhone, the camera capture stores a frame with one face and does not check liveness itself. See [Protect the enrolment step](guides/enrol.md#protect-the-enrolment-step).

## Recognition

### What is the recognition threshold?

A similarity of 0.35 on the SDK's 0 to 1 scale is the default bar for a match. YEO set it from internal testing, and your app does not need to choose one. Please talk to YEO before you consider changing anything related to it.

### What are the false accept and false reject rates?

They depend heavily on your users, devices and lighting, so we do not quote a single figure. YEO can discuss an evaluation on your target devices and conditions.

### Can identical twins unlock each other's account?

Face recognition, ours included, cannot reliably tell identical twins apart. For sensitive actions, use face sign-in together with your other security checks.

### What distance should the user hold the phone at?

About 30 to 40 cm, with the whole face in the picture. The framing hint says "move closer" or "move back" when needed.

### Does it need an internet connection?

Recognition and liveness run on the phone. On iOS no connection is needed. On Android, Google Play services must be present, and it needs a connection to download the face detection model the first time.

## Liveness

### Does the user have to blink, smile or turn their head?

No. Liveness is passive: the user just looks at the camera. Any head-turn prompts during enrolment are there to capture varied samples for recognition, not to test liveness.

### Why does it say "not live" for a moment at first?

The check judges a short sequence of frames, so it needs about a second, occasionally a few seconds, before its first answer. Show "Checking…" during that time rather than an error.

### Why does liveness flicker near a window?

Strong light behind the user underexposes the face and makes both recognition and liveness less certain. Ask the user to turn and face the light.

### Two people are in view. What happens?

Liveness is decided for one face, so the frame is not accepted until only one face remains. Show "One face at a time".

### Which value should our app use to decide liveness?

`isLive` or `status` in the plugins, `update.status` on iOS, and `faceState == Detected && isConfident()` in native Android. Do not compare `confidence` with a number: it is for display, and its scale can change between SDK versions.

### Can a photo or a video of the user fool it?

The check is designed to refuse common attacks, such as printed photos and photos or videos shown on a screen, and your app should accept recognition only together with liveness. Like any liveness system, it lowers the risk but cannot rule out every attack. Use it with your other security measures for high-value actions.

### Can we feed our own camera frames or stored photos?

Yes, the plugins and native SDKs accept frames from your own pipeline. Liveness only means something on a live camera stream, because frames replayed from storage cannot prove the person is present. Still-image matching runs recognition only.

## Data and storage

### Where is the enrolment stored, and is it encrypted?

On the device, in a place your app controls. The SDK does not encrypt it by default. For production, we recommend that your app encrypts it with a key in the iOS Keychain or Android Keystore and excludes it from cloud backup. See [Store the gallery](guides/store-the-gallery.md).

### Does any data leave the phone?

The SDK sends nothing. On Android, Google's ML Kit, which the SDK uses for face detection, sends Google its own usage metrics and downloads its model through Google Play services. Google states that images and detection results stay on the device. See [What leaves the device](data-and-privacy.md#what-leaves-the-device).

### Can we back up enrolments to the cloud or move them to a new phone?

We recommend keeping enrolments on the device and excluding them from cloud backup, because they are biometric data. On a new phone, the user should enrol again.

### Can we store galleries on our server?

The gallery is available as data you can store, but it is biometric data and must be protected in transit and at rest. Record which SDK version made each one, because a gallery from an older recognition model cannot be loaded by a newer one.

### How do we delete a user's face data?

Use the clear call for your platform and delete any copies you hold. See [How to delete it](guides/store-the-gallery.md#how-to-delete-it).

### How big is the saved enrolment?

About 4 KB for one sample, about 90 KB for a person with all 25 enrolment samples, and about 185 KB with both sets full.

## Upgrades

### Will users have to re-enrol after an app update?

Not for an update within the current versions. Moving from an older version with a different recognition model needs a one-time re-enrolment. See [Upgrade and re-enrol](guides/upgrade.md).

### What happens to an old enrolment after the upgrade?

The SDK refuses it safely. Your app sees that nobody is enrolled and should ask the user to enrol again. Delete the old file once the user has re-enrolled.

## Credential and setup

### Do we need a licence?

Yes. The SDK needs a credential from YEO. [Contact YEO](support.md).

### What happens if the credential is refused?

The SDK does not run. On Android you get an error your app can handle. On iOS the app closes during start-up. See [Before you start](get-started/before-you-start.md#get-your-credential).

### Does activating the SDK send anything to YEO?

No. The SDK makes no network connection of its own, including during activation.

### Does it work in a simulator or emulator?

Your app builds and runs there, but recognition needs a real phone and a real face.

### Does it work on Android phones without Google Play services?

No. These versions need Google Play services for face detection.

### Which operating systems are supported?

Android 7.0 (API 24) or later, and iOS 17.0 or later. See [Before you start](get-started/before-you-start.md#what-every-platform-needs).

## Security and compliance

### Does the SDK detect rooted or jailbroken phones?

No. The SDK does not check for rooted or jailbroken devices, emulators, hooking tools, screen capture or virtual cameras. Your app handles these, for example with Google Play Integrity or Apple App Attest. See [Device and app security](data-and-privacy.md#device-and-app-security).

### Are results signed?

No. Results are returned to your app as ordinary data structures. To rely on a result on your server, use your own attestation and secure channel.

### Does the SDK contain analytics or crash reporting?

No. On Android, ML Kit has its own usage metrics, as described in [What leaves the device](data-and-privacy.md#what-leaves-the-device).

### Are models updated over the air?

YEO's recognition and liveness models are not. They are bundled in the SDK and change when you ship an SDK update. On Android, Google Play services delivers and updates the separate face detection model.

### Does the SDK store face images on the device?

No, on either platform. The Android option `keepFaceImagesInTracker` has no effect in these versions.

### Can we wrap our app with a shielding or hardening product?

Yes. Those products change the app the camera runs in, so run your acceptance tests on a shielded build. See the [Go-live checklist](guides/go-live.md#testing).

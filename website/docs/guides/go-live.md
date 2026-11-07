# Go-live readiness checklist

**Get your face sign-in integration ready for production.**

This checklist helps you confirm that your YEO CFR SDK integration is configured correctly, thoroughly tested and ready for your users. Use it whether you are integrating for the first time or preparing an existing integration for release, and work through the sections that apply to you before you go live.

## 1. Access and credentials

Make sure your SDK credentials are configured securely and ready for production.

- [ ] Load your YEO credential from your app configuration or backend, not from source code, so that a renewal does not need a new app release.
- [ ] Confirm that the credential stays valid beyond your planned release date, and that someone is responsible for renewing it.
- [ ] Test your release build with the credential you will ship, on both iOS and Android.
- [ ] Store your Android repository address and login securely. Do not commit them to source control.

## 2. User enrolment

A reliable face sign-in starts with a properly completed enrolment.

- [ ] Users sign in through an existing method before they enrol their face. See [Protect the enrolment step](enrol.md#protect-the-enrolment-step).
- [ ] Use a guided capture with multiple samples rather than a single frame.
- [ ] Turn on face sign-in only after enrolment and a successful recognition check.
- [ ] Enrol each face under an opaque identifier rather than the user's real name or other personal information.
- [ ] Save the gallery as soon as the capture finishes, and handle a failed save.

## 3. Face recognition

Make sure your app handles recognition results correctly and consistently.

- [ ] Accept a result only when the face is confirmed as live and matches the expected person. See [Recognise a user](recognise.md).
- [ ] Require the result to hold for about a second, rather than relying on a single frame.
- [ ] Use the SDK's own decision: `status` or `isLive`, or `faceState == Detected && isConfident()` in native Android. Never compare `confidence` with a number.
- [ ] Always provide another way to sign in if face recognition cannot be completed.
- [ ] If you verify during a session, decide when it runs and what your app does when the person is no longer present. See [Verify throughout a session](continuous.md).
- [ ] For sensitive or high-value actions, add further authentication suited to your security requirements.

## 4. User experience

Clear guidance helps users complete face sign-in successfully.

- [ ] Show a status such as "Checking…" during the first seconds. Do not show a failure while the SDK is still confirming liveness.
- [ ] Make sure framing hints reach the user. See [Coach your users](coach-users.md).
- [ ] Allow 5 to 10 seconds for each attempt, then show helpful tips and an option to retry.
- [ ] Write a clear camera permission message that explains why your app needs the camera.
- [ ] Handle a refused camera permission gracefully, including offering another way to sign in.
- [ ] Lock the face recognition screen to portrait. Capacitor and Cordova require this.

## 5. Data protection and privacy

Face data needs protection throughout its lifecycle. Consider these recommendations alongside your organisation's data protection and privacy requirements. See [Store the gallery](store-the-gallery.md) for how to do each one.

- [ ] Encrypt the gallery with keys held in the iOS Keychain or Android Keystore.
- [ ] Exclude the gallery from cloud backups and device-to-device transfers.
- [ ] When a user turns off face sign-in, delete the gallery, including any copies your app manages.
- [ ] Make sure your privacy notice explains how biometric data is processed and stored on the device.
- [ ] On Android, make sure your privacy notice and your Google Play Data safety form reflect Google's ML Kit data disclosure. See [What leaves the device](../data-and-privacy.md#what-leaves-the-device).

## 6. Application and device security

The SDK provides face recognition and liveness. The overall security of your sign-in also depends on how your app and backend handle the results. Consider these points as part of your security assessment. See [Device and app security](../data-and-privacy.md#device-and-app-security).

- [ ] Decide how your app handles rooted, jailbroken or otherwise tampered devices.
- [ ] Decide whether to restrict screen capture and recording on the face recognition screen.
- [ ] Make sure your backend does not rely on a result from the app as its only proof of authentication.
- [ ] Apply suitable authentication, authorisation and further verification to sensitive operations.

## 7. Device compatibility and testing { #testing }

Before you release to users, check your integration under realistic conditions.

- [ ] Test on real iPhones and Android phones, not only on simulators or emulators.
- [ ] Test in different lighting, including backlight and dim rooms.
- [ ] Test with users wearing glasses, and with more than one face in view.
- [ ] Test leaving the face recognition screen, sending the app to the background and coming back.
- [ ] If your app uses a shielding or hardening product, run acceptance tests on a shielded build of the same commit. Develop and measure recognition on unshielded builds first, and expect screenshots of the face recognition screen to come out black when shielding is on.
- [ ] Android: check that your release build meets Google Play's 16 KB memory page size requirement, using APK Analyzer or this command:

    ```bash
    zipalign -c -P 16 -v 4 app-release.apk
    ```

- [ ] Android: test the first use on a real phone with Google Play services and a network connection, because the face detection model downloads at that point.

## 8. Support and upgrades

Prepare your own team for launch day.

- [ ] Your support team has the [coaching tips](coach-users.md).
- [ ] If this is an upgrade from an older SDK version, you have a [re-enrolment plan](upgrade.md).

## Ready for production?

Once you have completed the checks that apply to you, your integration is ready for your own production release assessment.

YEO provides the SDK and its face recognition and liveness capabilities. Your app remains responsible for the surrounding sign-in flow, credential management, data storage, privacy notices, backend validation and overall application security.

**Need help?** If you have questions about integration, configuration or production readiness before you go live, [contact the YEO team](../support.md).

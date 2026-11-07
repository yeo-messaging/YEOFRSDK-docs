# Go-live checklist

Work through this list before you release face sign-in to your users.

## Access and credential

- [ ] The credential from YEO is loaded from configuration or your backend, not hard-coded.
- [ ] The credential's expiry date is beyond your release date, and someone owns its renewal.
- [ ] You have tested the release build with the real credential on both platforms.
- [ ] Android repository details are in secret storage, not in source control.

## Enrolment

- [ ] Enrolment runs only after the user has signed in by another means.
- [ ] You use a guided capture with several samples, not one frame.
- [ ] A successful recognition is required before face sign-in is turned on.
- [ ] The enrolled name is an opaque ID, not a real name.
- [ ] The gallery is saved as soon as the capture finishes, and a failed save is handled.

## Recognition

- [ ] Your app accepts only when the face is live and has the expected name.
- [ ] The result must hold for about a second, not one frame.
- [ ] No code compares `confidence` with a number.
- [ ] A fallback sign-in is always available.
- [ ] High-value actions use more than face sign-in.

## Screens

- [ ] You show "Checking…" during the first seconds, never a failure.
- [ ] Framing hints reach the user.
- [ ] Each attempt allows 5 to 10 seconds, then shows tips and a retry.
- [ ] The camera permission text is written for your users, and refusal is handled.
- [ ] The face screen is locked to portrait, as Capacitor and Cordova require.

## Data and privacy

- [ ] The gallery is encrypted by your app, with a Keychain or Keystore key.
- [ ] The gallery is excluded from cloud backup and device transfer.
- [ ] Turning off face sign-in deletes the gallery, including your own copies.
- [ ] Your privacy notice covers biometric processing on the device.
- [ ] On Android, your privacy notice and your Google Play Data safety form cover Google's ML Kit usage metrics. See [What leaves the device](../data-and-privacy.md#what-leaves-the-device).

## Device and app security

- [ ] You have decided how your app handles rooted, jailbroken or tampered devices. The SDK does not check this.
- [ ] You have decided whether to block screen capture on the face screen. The SDK does not do this.
- [ ] Your backend does not treat a result from the app as proof on its own. Results are not signed by the SDK.

## Testing

- [ ] You have tested on real iPhones and real Android phones, not simulators.
- [ ] You have tested in backlight, dim light, with glasses, and with two faces in view.
- [ ] You have tested leaving the face screen, sending the app to the background and coming back.
- [ ] If your app is wrapped in a shielding or hardening product, you have run acceptance on a shielded build of the same commit. Develop and measure on unshielded builds, and expect screenshots of the face screen to come out black.
- [ ] Android: Google Play's 16 KB page size check passes. Use APK Analyzer or `zipalign -c -P 16 -v 4 app-release.apk`.
- [ ] Android: you have tested on a phone with Google Play services and a network connection for the first use.

## Support

- [ ] Your support team has the [coaching tips](coach-users.md).
- [ ] If this is an upgrade, you have a [re-enrolment plan](upgrade.md).

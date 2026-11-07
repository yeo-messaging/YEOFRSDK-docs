# Before you start

Three things save the most time: get your access from YEO early, test on real phones, and decide where the saved enrolment will live.

## Get your credential

The SDK needs a credential from YEO, and it will not run without one. [Contact YEO support](../support.md) to get one.

Handle it like this:

- **Keep it out of your source code.** Load it from your build configuration or your backend, so that a new credential does not need a new app release.
- **Cover your whole test period.** Credentials have an expiry date. Ask YEO for the next one before the current one runs out.
- **Check it before every release, on both platforms.** A refused credential does not behave the same everywhere:

| Platform | What happens when the credential is refused |
|---|---|
| Android | `YEOFRSDK.init` throws an exception. |
| iOS | The app closes during start-up. You cannot catch this, and it looks like a crash. |
| React Native, Capacitor, Cordova | On Android the call rejects with an error. On iOS the app closes, as above. |
| Flutter | On Android `YeoFR.init` throws a `PlatformException` with code `INIT_ERROR`. On iOS the app closes, as above. |

!!! warning "If your iOS app closes when the face screen opens"
    Check the credential first.

## Get access to the Android package

The Android SDK is in a private Maven repository. YEO support gives you its address and a login. You need these for Android builds on every platform, including React Native, Flutter, Capacitor and Cordova.

Keep the address and login out of source control. Put them in `~/.gradle/gradle.properties` on developer machines and in secret environment variables on your build server.

The iOS SDK is distributed from a public Swift package, so it needs no login.

## What every platform needs

| | Android | iOS |
|---|---|---|
| **Operating system** | Android 7.0 (API 24) or later | iOS 17.0 or later |
| **Device** | A real phone with Google Play services | A real iPhone. A TrueDepth camera is recommended. |
| **Build tools** | `compileSdk` 36 and Android Gradle Plugin 8.9.1 or later | Swift 5.9 or later. Bridge plugins have their own Xcode requirement: see their pages. |
| **Permission** | Camera | Camera (`NSCameraUsageDescription`) |

!!! info "Test on real phones"
    Apps build and run in the simulator or emulator, but recognition needs a real device and a real face.

!!! info "Google Play services on Android"
    Face detection on Android is delivered through Google Play services, and the face model downloads on first use. Phones without Google Play services are not supported, and the first use needs a network connection.

## Decide three things early

1. **Where the saved enrolment lives.** On the phone, encrypted by your app, is what we recommend. See [Store the gallery](../guides/store-the-gallery.md).
2. **How the user gets back in without their face.** Face sign-in needs a fallback such as a PIN or a password, for failed attempts and for moments when the user must enrol again.
3. **One person per app installation.** This is the setup we recommend. If you need several people on one installation, talk to YEO first, because identity handling differs by platform.

## Next step

Pick your platform: [Android](android.md), [iOS](ios.md), [React Native](react-native.md), [Flutter](flutter.md), [Capacitor](capacitor.md) or [Cordova](cordova.md).

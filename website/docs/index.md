---
title: YEO CFR SDK
hide:
  - navigation
  - toc
---

<div class="yeo-hero" markdown>

# Face recognition and liveness, on the phone

The YEO CFR SDK lets your app enrol a user's face, recognise that user later, and check that a real person is in front of the camera. All of this runs on the phone. These pages show you how to build it, what we recommend, and what your security and compliance teams will want to know.

[Get started](get-started/index.md){ .md-button .md-button--primary }
[Read the FAQ](faq.md){ .md-button }

</div>

## Choose your platform

<div class="grid cards" markdown>

-   :material-android: **Android**

    ---

    Native Kotlin or Java apps.

    [Get started on Android](get-started/android.md)

-   :material-apple: **iOS**

    ---

    Native Swift apps on iOS 17 or later.

    [Get started on iOS](get-started/ios.md)

-   :material-react: **React Native**

    ---

    One JavaScript API with a ready-made camera view.

    [Get started with React Native](get-started/react-native.md)

-   :simple-flutter: **Flutter**

    ---

    A Dart API that works with your own camera stream.

    [Get started with Flutter](get-started/flutter.md)

-   :material-lightning-bolt: **Capacitor**

    ---

    Web apps in a Capacitor shell, with or without Ionic.

    [Get started with Capacitor](get-started/capacitor.md)

-   :material-cellphone-link: **Cordova**

    ---

    Web apps on Cordova, including Ionic apps.

    [Get started with Cordova](get-started/cordova.md)

</div>

## Find what you need

<div class="grid cards" markdown>

-   :material-account-plus: **Enrol a user**

    ---

    Capture a face well the first time, and what to tell the user.

    [Enrolment guide](guides/enrol.md)

-   :material-account-check: **Recognise a user**

    ---

    The one rule your app should use to accept a face.

    [Recognition guide](guides/recognise.md)

-   :material-shield-lock: **Data and privacy**

    ---

    What is stored, what leaves the phone, and who encrypts what.

    [Data and privacy](data-and-privacy.md)

-   :material-clipboard-check: **Go-live checklist**

    ---

    What to check before you release to your users.

    [Go-live checklist](guides/go-live.md)

</div>

## The short version

- **On the device.** Enrolment, recognition and liveness run on the phone. The SDK does not send faces, face samples or results anywhere.
- **No photos kept.** The SDK saves numeric face samples, not images.
- **Your app holds the data.** Encryption, backup rules and deletion of the saved enrolment are your app's decisions. [We explain how](guides/store-the-gallery.md).
- **Nothing for the user to perform.** The liveness check is passive. The user just looks at the camera.
- **A credential is required.** The SDK needs a credential from YEO. [Contact YEO](get-started/before-you-start.md#get-your-credential) to get one.

!!! note "Which versions these pages describe"
    Android 0.2.29, iOS 0.11.2, React Native 0.5.2, Flutter 0.5.1, Capacitor 0.5.0 and Cordova 0.5.0. See [Published versions](reference/versions.md).

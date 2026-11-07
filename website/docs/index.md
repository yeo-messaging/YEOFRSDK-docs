---
template: home.html
title: YEO CFR SDK
hide:
  - navigation
  - toc
  - footer
hero:
  eyebrow: YEO CFR SDK documentation
  title: Build continuous facial recognition into your app
  text: Enrol a user's face and confirm that the right person is present, at sign-in and throughout the session. It all runs on the phone. These pages show you how to build it, what we recommend, and what your security and compliance teams will want to know.
platforms:
  - name: iOS
    key: ios
    icon: simple/ios
    text: Native Swift apps on iOS 17 or later.
    url: get-started/ios/
  - name: Android
    key: android
    icon: simple/android
    text: Native Kotlin or Java apps.
    url: get-started/android/
  - name: React Native
    key: react
    icon: simple/react
    text: One JavaScript API with a ready-made camera view.
    url: get-started/react-native/
  - name: Flutter
    key: flutter
    icon: simple/flutter
    text: A Dart API that works with your own camera stream.
    url: get-started/flutter/
  - name: Capacitor
    key: capacitor
    icon: simple/capacitor
    text: Web apps in a Capacitor shell, with or without Ionic.
    url: get-started/capacitor/
  - name: Cordova
    key: cordova
    icon: simple/apachecordova
    text: Web apps on Cordova, including Ionic apps.
    url: get-started/cordova/
guides:
  - name: Enrol a user
    icon: material/account-plus
    text: Capture a face well the first time, and what to tell the user.
    url: guides/enrol/
  - name: Recognise a user
    icon: material/account-check
    text: The one rule your app should use to accept a face.
    url: guides/recognise/
  - name: Verify throughout a session
    icon: material/account-clock
    text: Keep confirming who is present, and decide what happens when they leave.
    url: guides/continuous/
  - name: Data and privacy
    icon: material/shield-lock
    text: What is stored, what leaves the phone, and who encrypts what.
    url: data-and-privacy/
  - name: Go-live checklist
    icon: material/clipboard-check
    text: What to check before you release to your users.
    url: guides/go-live/
  - name: Support
    icon: material/lifebuoy
    text: How to reach the YEO team, and more resources.
    url: support/
---

## The short version

<div class="grid cards" markdown>

-   **Continuous**

    The SDK reports on every analysed camera frame, so your app can check who is present at any point in a session, not only at login.

-   **On the device**

    Enrolment, recognition and liveness run on the phone. The SDK does not send faces, face samples or results anywhere.

-   **No photos kept**

    The SDK saves numeric face samples, not images.

-   **Your app holds the data**

    Encryption, backup rules and deletion of the saved enrolment are your app's decisions. [We explain how](guides/store-the-gallery.md).

-   **Nothing for the user to perform**

    The liveness check is passive. The user just looks at the camera.

-   **A credential is required**

    The SDK needs a credential from YEO. [Contact YEO](support.md) to get one.

</div>

!!! note "Which versions these pages describe"
    Android 0.2.29, iOS 0.11.2, React Native 0.5.2, Flutter 0.5.1, Capacitor 0.5.0 and Cordova 0.5.0. See [Published versions](reference/versions.md).

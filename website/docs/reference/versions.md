# Published versions

These docs always describe the current versions below. For earlier versions, see [What's new](whats-new.md).

| Product | Version | Android SDK inside | iOS SDK inside | How you get it |
|---|---|---|---|---|
| Android SDK | **0.2.29** | | | `com.yeomessaging:yeofr-android`, from YEO's private Maven repository |
| iOS SDK | **0.11.2** | | | Swift package, from [YEOFR-SPM](https://github.com/yeo-messaging/YEOFR-SPM) |
| React Native | **0.5.2** | 0.2.29 | 0.11.0 | `@yeomessagingcom/react-native-yeofr` on npm |
| Flutter | **0.5.1** | 0.2.29 | 0.11.0 | `yeofr_flutter`, from YEO's Git repository at tag `v0.5.1` |
| Capacitor | **0.5.0** | 0.2.29 | 0.11.2 | `@yeomessagingcom/capacitor-yeofr` on npm |
| Cordova | **0.5.0** | 0.2.29 | 0.11.2 | `@yeomessagingcom/cordova-plugin-yeofr` on npm |

Plugins pin the SDK versions they were built with, so use the versions in this table. Flutter requires the exact Android version.

## Minimum requirements

| | Android | iOS |
|---|---|---|
| **Operating system** | Android 7.0 (API 24) | iOS 17.0 |
| **Build** | `compileSdk` 36, Android Gradle Plugin 8.9.1 or later | Swift 5.9 or later. Capacitor and Cordova need Xcode 26 or later. |
| **On the device** | Google Play services | Front camera. TrueDepth is recommended for the depth check. |

| Plugin | Also needs |
|---|---|
| Flutter | Flutter 3.10 or later |
| Capacitor | Capacitor 7 or 8 |
| Cordova | `cordova-android` 13 or 14, `cordova-ios` 7 or 8 |

## Release notes

See [What's new](whats-new.md) for what changed in each version, and [API references](api.md) for the full reference.

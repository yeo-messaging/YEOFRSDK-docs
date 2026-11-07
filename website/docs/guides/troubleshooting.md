# Troubleshooting

## The app or build

| Symptom | Likely cause | What to do |
|---|---|---|
| The iOS app closes when the face screen opens | The credential was refused, or `NSCameraUsageDescription` is missing | Check the credential first, then `Info.plist`. iOS ends any app that opens the camera without the text. |
| Android `init` throws "Invalid licence code", or a call rejects with an activation error | The credential is missing, unknown or expired | Ask YEO for a current credential |
| `Could not find com.yeomessaging:yeofr-android:0.2.29` | The YEO repository is missing from your app's build | Add the repository block for your platform. It must be in your app's build, not only in a dependency's. |
| `401` from the repository | Wrong or missing login | Check the login from YEO in `gradle.properties` or your environment variables |
| A build error about `compileSdk` or Android Gradle Plugin | The project is on older tools | Use `compileSdk` 36 and Android Gradle Plugin 8.9.1 or later |
| Google Play reports native libraries that are not 16 KB aligned | An older TensorFlow Lite is in your build, or Android Gradle Plugin is too old | Use SDK 0.2.29, remove `org.tensorflow:tensorflow-lite` 2.14 or older, and use Android Gradle Plugin 8.5.1 or later |
| iOS package resolution fails | The deployment target is below 17.0 | Raise it to 17.0 |
| A simulator build fails with `unable to resolve module dependency: 'YEOFR'` | The package has no Intel simulator slice | Exclude `x86_64` from simulator builds, and use an Apple-silicon Mac |

## The camera and results

| Symptom | Likely cause | What to do |
|---|---|---|
| Results always say no face on a simulator or emulator | Neither provides a real face | Test on a real phone |
| Results stay at no face on an Android phone | Google Play services is missing or out of date, or the face model has not downloaded yet | Update Google Play services and connect to a network for the first use |
| Camera does not start on Android | The permission is missing from your manifest, or has not been requested | Declare `CAMERA` and request it at runtime |
| Camera refused on iOS after the first time | iOS asks only once | Ask the user to allow the camera in Settings |
| Capacitor or Cordova: the camera runs but the screen is white or a solid colour | Something opaque covers the preview | Make the page transparent where the camera should show, as in the [Capacitor guide](https://yeo-messaging.github.io/YEOFRSDK-docs/capacitor/) or [Cordova guide](https://yeo-messaging.github.io/YEOFRSDK-docs/cordova/) |
| Capacitor or Cordova: `alreadyRunning` | Two pages started the camera, or a start had no stop | Keep one camera session per app, in one shared service |

## Enrolment and recognition

| Symptom | Likely cause | What to do |
|---|---|---|
| "Not live" for the first moment | The check is warming up | Show "Checking…" |
| Liveness flickers near a window | Strong light behind the user | Turn to face the light |
| The right person is not recognised | Different light from enrolment, or a single-frame enrolment | See [Coach your users](coach-users.md), or re-enrol with a guided capture |
| Recognition does not improve over time | Recent samples were not saved | Save the gallery after sign-in sessions. See [How it works](how-it-works.md#getting-better-over-time). |
| The enrolment is gone after an update | The app moved to a new recognition model | [Plan a one-time re-enrolment](upgrade.md) |
| `loadTracker` returns a negative code or a failure | The gallery came from an older version, or the bytes are damaged | Discard it and ask the user to enrol again |

## More help

[Contact YEO support](../support.md). Tell us the SDK version, the platform and phone model, what you expected and what happened, and any error code. See [what to include](../support.md#what-to-include).

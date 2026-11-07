# Get started on Android

Version 0.2.29. You provide the camera frames, and the SDK gives you a result for each one.

## 1. Add the SDK

YEO support gives you the repository address and a login. Keep them outside source control, for example in `~/.gradle/gradle.properties`.

```kotlin
repositories {
    maven {
        url = uri(providers.gradleProperty("yeoMavenUrl").get())
        credentials {
            username = providers.gradleProperty("yeoMavenUser").get()
            password = providers.gradleProperty("yeoMavenPassword").get()
        }
    }
}
dependencies {
    implementation("com.yeomessaging:yeofr-android:0.2.29")
}
```

Your app needs `minSdk` 24, `compileSdk` 36 and Android Gradle Plugin 8.9.1 or later.

## 2. Start the SDK

```kotlin
YEOFRSDK.init(context, code = credentialFromYeo)
```

`init` throws an exception if YEO's credential is missing or refused. See [Before you start](before-you-start.md#get-your-credential).

## 3. Send camera frames

Build one `ImageFrameParameters` per frame and call `detectFaces` from a single thread. The fastest hand-off is raw ARGB32 from an upright bitmap.

```kotlin
val buffer = ByteBuffer.allocate(bitmap.rowBytes * bitmap.height)
bitmap.copyPixelsToBuffer(buffer)
val params = ImageFrameParameters(
    height = bitmap.height,
    width = bitmap.width,
    scanline = bitmap.rowBytes,
    ratio = 1.0,
    buffer = buffer.array(),
    bufferSize = buffer.capacity(),
    uuid = UUID.randomUUID(),
)
val result = YEOFRSDK.detectFaces(params)
```

## 4. Enrol the user

Enrol only from a frame with one live face. Each `enroll` call adds one sample, so call it several times across a short capture. See [Enrol a user](../guides/enrol.md).

```kotlin
if (result.faceState == FaceRecognitionState.Detected && result.isConfident()) {
    val faceId = result.faceIDs.keys.first()
    YEOFRSDK.enroll(faceId, "user-id", learnFromCurrentFrame = true)   // 0 means success
}
```

## 5. Save the gallery

The SDK hands the enrolment to your app as bytes, and it never writes them to disk itself. Your app decides where they go and encrypts them. See [Store the gallery](../guides/store-the-gallery.md).

```kotlin
val gallery: ByteArray? = YEOFRSDK.faceRecognitionTrackerData()   // save this
val code = YEOFRSDK.loadTracker(gallery)                          // negative means refused
```

## 6. Recognise the user

```kotlin
val live = result.faceState == FaceRecognitionState.Detected && result.isConfident()
val recognised = live && result.faceIDs.values.any { it.isNotEmpty() }
```

Accept only once this has held for about a second. See [Recognise a user](../guides/recognise.md).

## Keep going

- Google Play's 16 KB page size requirement is met by 0.2.29. Do not add `org.tensorflow:tensorflow-lite` 2.14 or older to your app, because it brings the problem back.
- [Android API reference](https://yeo-messaging.github.io/yeofrsdk-android-docs/)
- [Go-live checklist](../guides/go-live.md)

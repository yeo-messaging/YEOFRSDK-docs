# Store the gallery

The gallery holds your user's face samples. Treat it as biometric data. The SDK produces it, and **protecting it is your app's job**: the SDK does not encrypt it for you in any default setup.

## What we recommend

1. **Keep it on the device.** Keep it in the app's private storage and do not sync it.
2. **Encrypt it yourself.** Use a key held in the iOS Keychain or the Android Keystore.
3. **Exclude it from cloud backup and device-to-device transfer.**
4. **Delete it when the user turns off face sign-in.** Delete your own copies as well as the SDK's.
5. **Use an opaque ID as the enrolled name,** so the file holds no real name.

## What is in it

- The names you enrolled, and when.
- Numeric face samples: up to 25 from enrolment and up to 25 recent ones, per person.
- The format version and the recognition model's version.

There are no photos or video.

| Gallery | Approximate size |
|---|---|
| One sample | 4 KB |
| One person with all 25 enrolment samples | 90 KB |
| One person with both sets full | 185 KB |

## What happens by default

| Platform | Where it goes by default | Encrypted | In device backups |
|---|---|---|---|
| Android | Nowhere. The SDK gives your app the bytes. | Your choice | Your choice |
| iOS with `FaceTrustService` | A file in the app's Documents folder, saved after enrolment. You can pass another location as `trackerURL`. | No | Yes, unless you exclude it |
| React Native | A private file when you call `saveTrackerToDisk()`: Android files folder, iOS Application Support | No | Unless you exclude it, it follows your app's backup rules |
| Flutter | Nowhere. `saveTracker()` gives your app the bytes. | Your choice | Your choice |
| Capacitor, Cordova | A private file, saved automatically: Android no-backup folder, iOS Application Support with file protection | No | No |

## How to encrypt it

=== "Android"

    Encrypt the bytes with a key from the Android Keystore, for example AES-GCM, and store the result in your app's private storage.

    ```kotlin
    val gallery: ByteArray? = YEOFRSDK.faceRecognitionTrackerData()
    // encrypt with your Keystore key, then store

    // On the next launch, after init:
    val code = YEOFRSDK.loadTracker(decryptedBytes)   // negative means refused
    ```

=== "iOS"

    You can give the SDK your own `Cryptor`, for example one built on a CryptoKit authenticated cipher, and read and write the gallery through it.

    ```swift
    let sdk = try YEOFRSDK(pilotUnlockCode: credentialFromYeo,
                           encryption: .custom(MyCryptor()))

    // Save
    if let blob = try sdk.encryptedFaceRecognitionTrackerData() {
        try blob.serialized().write(to: url, options: .atomic)
    }

    // Load
    try sdk.loadTracker(from: EncryptedBlob.parse(Data(contentsOf: url)))
    ```

    !!! warning "`FaceTrustService` still writes an unencrypted file"
        Handing an encrypting SDK to `FaceTrustService(sdk:)` does not encrypt its saved file. For encrypted storage, your app must own the saving, and it must also remove the service's own file. `Examples/FRPilot` in the Swift package shows a worked example.

=== "React Native"

    Use `getTrackerData()` and `loadTracker()` instead of the disk helpers. Encrypt the base64 string with a key from the Keychain or Keystore, and store it yourself.

    ```ts
    const data = await getTrackerData();           // encrypt, then store
    const result = await loadTracker(decrypted);   // use isOK(result) to check
    ```

=== "Flutter"

    Encrypt the bytes with a key from the Keychain or Keystore, and store them yourself.

    ```dart
    final bytes = await YeoFR.saveTracker();   // encrypt, then store
    await YeoFR.loadTracker(decrypted);        // negative means refused
    ```

=== "Capacitor"

    Read the template once `registered` has arrived, store it encrypted, and put it back before `start()`.

    ```ts
    const data = await Yeofr.getTrackerData();
    // encrypt, then store. Later:
    await Yeofr.loadTracker(decrypted);
    ```

    The plugin's default store still keeps a copy until you call `clearTracker()`.

=== "Cordova"

    Read the template once `registered` has arrived, store it encrypted, and put it back before `start()`.

    ```ts
    const data = await cordova.plugins.Yeofr.getTrackerData();
    // encrypt, then store. Later:
    await cordova.plugins.Yeofr.loadTracker(decrypted);
    ```

    The plugin's default store still keeps a copy until you call `clearTracker()`.

## How to keep it out of backups

**Android.** Set `android:allowBackup="false"` and add `android:dataExtractionRules` that exclude the file. These cover cloud backup and device transfer.

**iOS.** Store the file outside Documents, for example in Application Support, and set `isExcludedFromBackup` on it. Turn on the Data Protection capability for the strongest protection your app can tolerate.

## How to delete it

| Platform | How to remove the enrolment |
|---|---|
| Android | `YEOFRSDK.clearTracker()`, then delete your stored copy |
| iOS with `FaceTrustService` | `clearEnrollment()` clears memory and removes the service's file. Delete any copy you hold yourself. |
| React Native | `clearTracker()`, then `saveTrackerToDisk()` to replace the saved copy. Confirm in your app that the next launch finds nobody enrolled. |
| Flutter | `YeoFR.clearTracker()`, then delete your stored copy |
| Capacitor, Cordova | `clearTracker()` removes the stored face. Delete any copy you hold yourself. |

Uninstalling the app removes files in its private storage. Copies on your server or in a backup are yours to remove.

## Can we keep galleries on our server?

You can. The gallery is available as bytes or as text, so it can be sent and stored as an opaque blob. It is still biometric data, so protect it in transit and at rest, and record which SDK version made it, because a gallery from an older recognition model cannot be loaded by a newer one.

## New phones

Ask the user to enrol again on the new phone. It takes about a minute and gives the best results with that phone's camera.

# Recognise a user

The SDK reports on every analysed frame, and your app decides when it has seen enough.

## The rule we recommend

Accept the user only when **all** of these are true:

1. The SDK says the face is live.
2. The SDK names the person you expect.
3. Both have held together for about a second.

Do not accept on a single frame, and do not build your own rule from the numbers.

Check that the recognised name is the person you expect, even when the status already says `trusted`. Before you start recognition, also confirm that someone is enrolled.

## The rule on each platform

=== "Android"

    ```kotlin
    val live = result.faceState == FaceRecognitionState.Detected && result.isConfident()
    val accepted = live && result.faceIDs.values.contains(expectedUser)
    ```

    `faceState == Detected` matters. With a second face in view, the last verdict is held for up to 1.5 seconds, so `isConfident()` alone can still be true.

=== "iOS"

    ```swift
    for await update in service.updates() {
        let accepted = update.status == .trusted && update.matchedName == expectedUser
    }
    ```

    `.trusted` combines liveness with the identity check. On phones with a TrueDepth camera, it also includes depth.

    To limit recognition to one person when several are enrolled, use `setActiveRecognitionIdentity(_:)`.

=== "React Native"

    ```ts
    if (event.type === 'result') {
      const accepted = event.status === 'trusted' && event.matchedName === expectedUser;
    }
    ```

=== "Flutter"

    ```dart
    final accepted = result.isLive && result.faceIDs.firstOrNull?.id == expectedUser;
    ```

    `isLive` is already false when no face, or more than one face, is in view.

=== "Capacitor"

    ```ts
    await Yeofr.addListener('result', (r) => {
      const accepted = r.status === 'trusted' && r.matchedName === expectedUser;
    });
    ```

=== "Cordova"

    ```ts
    await cordova.plugins.Yeofr.addListener('result', (r) => {
      const accepted = r.status === 'trusted' && r.matchedName === expectedUser;
    });
    ```

## What the statuses mean

iOS, React Native, Capacitor and Cordova report one of four statuses.

| Status | Meaning | What to show |
|---|---|---|
| `noFace` | No face in view | "Look at the camera" |
| `badFraming` | A face is in view but cannot be used: too far, too close or badly placed | The framing hint, or "Adjust your position" |
| `notTrusted` | A face that has not passed liveness or is not the enrolled person. With more than one face in view, some platforms report this instead of `badFraming`. | See below |
| `trusted` | Live, and recognised | Your success state |

`notTrusted` covers several situations. You can tell them apart with `isLive` and `matchedName`, and with the framing hint for a second face:

| Situation | How to tell | What to show |
|---|---|---|
| More than one face | `detectedCount` is above 1 | "One face at a time" |
| Liveness not confirmed | `isLive` is false, whether or not `matchedName` is set | "Checking…" |
| Someone else | `isLive` is true, `matchedName` is empty, and someone is enrolled | "Not recognised" |
| Nobody enrolled yet | `isLive` is true, `matchedName` is empty, and nobody is enrolled | Offer enrolment |

## Timing

Within about a second of looking at the camera, a real enrolled user is normally recognised and confirmed as live. In backlight or dim light it can take longer. Allow 5 to 10 seconds per attempt. See [Coach your users](coach-users.md).

## For sensitive actions

- **High-value actions.** Use face sign-in as one factor, together with your other security checks.
- **One device, one person.** Keep each installation to the person who enrolled on it.

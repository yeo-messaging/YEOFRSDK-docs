# Enrol a user

A good enrolment is the best thing you can do for recognition later. This guide covers what to capture, how to protect the step, and how to do it on each platform.

## What we recommend

- **Use a guided capture.** Capture several samples with small head turns, and if possible in more than one light. One frame is enough to get started, but it can fail later when the light changes.
- **Confirm who the user is first.** Run enrolment only after the user has signed in by another means.
- **Confirm the result.** After the capture, require one successful recognition before you turn on face sign-in.
- **Use an opaque ID as the name.** The name you pass is saved in the gallery, so use an internal customer ID rather than a real name.
- **Keep to one person per app installation.** If you need several people, talk to YEO first.
- **Save straight away.** Save the gallery as soon as the capture finishes, and offer a retry if saving fails.

## How many samples

| Capture style | Samples | Typical time |
|---|---|---|
| Quick capture | One good frame | About a second |
| Guided capture (recommended for production) | Up to 25, from several head angles | About 5 seconds to around a minute, depending on your prompts |

More varied samples make recognition more reliable, especially when the light changes.

## Protect the enrolment step

Enrolment decides whose face is trusted later.

1. Run enrolment only inside a session you already trust, after your own sign-in check.
2. After the capture, require a `trusted` result with the expected name before you turn on face sign-in. `trusted` includes liveness.
3. If that check does not pass, clear the enrolment and start again.

In native Android and in Flutter your app calls `enroll`, so check liveness first, as the samples below do.

## How to enrol on each platform

=== "Android"

    Call `enroll` on each frame you want to keep. Each call adds one sample under the same ID, up to 25.

    ```kotlin
    if (result.faceState == FaceRecognitionState.Detected && result.isConfident()) {
        val faceId = result.faceIDs.keys.first()
        YEOFRSDK.enroll(faceId, "user-id", learnFromCurrentFrame = true)   // 0 means success
    }
    ```

    Space the calls a little apart, prompt small head turns while they run, and save when you are done with `faceRecognitionTrackerData()`.

=== "iOS"

    Each `enroll(name:)` adds one sample, up to 25, and saves the file for you.

    ```swift
    for await step in service.enroll(name: "user-id") {
        // step.phase: .capturing (show step.prompt), then .completed or .failed
    }
    ```

    Call it again as the user turns slightly or moves to other light. You can also enrol from photos you hold with `enroll(name:images:)`. For a full walk through five head positions from your own camera loop, see `PoseWalkEnrollment` in the [API reference](https://yeo-messaging.github.io/YEOFRSDK-docs/0.11.2/documentation/yeofr/).

=== "React Native"

    Set `enrolName`, wait for `registered`, then save.

    ```tsx
    <YeofrCamera
      enrolName={enrolling ? 'user-id' : undefined}
      enrolPoses={['front', 'yawLeft', 'yawRight', 'pitchUp', 'pitchDown']}
      onEvent={(e) => handle(e.nativeEvent)}
    />
    ```

    ```ts
    if (event.type === 'registered') await saveTrackerToDisk();
    ```

    - **Android:** the poses make a paced walk of at least five seconds.
    - **iOS:** the capture is a single front capture, and `enrolProgress` reports one `front` pose.
    - **Cancelling:** to abandon a capture, reload the saved gallery or clear it.

=== "Flutter"

    Your app runs the capture loop. Each `enroll` adds one sample, up to 25, so call it on every frame you want to keep.

    ```dart
    if (result.faceState == FaceState.detected && result.isLive) {
      final faceId = result.faceIDs.firstOrNull?.index;
      if (faceId != null) await YeoFR.enroll(faceId, 'user-id');   // 0 means success
    }

    // When the capture is finished:
    final bytes = await YeoFR.saveTracker();
    if (bytes != null) await saveTrackerToStorage(bytes);
    ```

    Space the calls a little apart and prompt small head turns while they run.

=== "Capacitor"

    ```ts
    await Yeofr.enrol({
      name: customerId,
      poses: ['front', 'yawLeft', 'yawRight', 'pitchUp', 'pitchDown'],
    });
    ```

    Show progress from `enrolProgress` events using `posesAccepted` and `posesPlanned`. The capture is finished when `registered` arrives.

    - The poses are prompts. Neither platform measures head position, so they guide the user but do not block the capture.
    - **Android:** each pose is prompted for 3 seconds before its frames count.
    - **iOS:** there is one capture per pose, with a short pause before each. Sending the app to the background ends the capture, so start it again when the user returns.
    - A capture replaces the enrolled identity. If it is cut short, the previous enrolment is put back.

=== "Cordova"

    ```ts
    await cordova.plugins.Yeofr.enrol({
      name: customerId,
      poses: ['front', 'yawLeft', 'yawRight', 'pitchUp', 'pitchDown'],
    });
    ```

    Show progress from `enrolProgress` events using `posesAccepted` and `posesPlanned`. The capture is finished when `registered` arrives.

    - The poses are prompts. Neither platform measures head position, so they guide the user but do not block the capture.
    - **Android:** each pose is prompted for 3 seconds before its frames count.
    - **iOS:** there is one capture per pose, with a short pause before each. Sending the app to the background ends the capture, so start it again when the user returns.
    - A capture replaces the enrolled identity. If it is cut short, the previous enrolment is put back.

## What to tell the user

**Before starting**

- Hold the phone upright, at eye level, and use the front camera.
- Face a window or a lamp. Do not sit with a bright light behind your head.
- Remove sunglasses, hats, hoods and face masks. Clear glasses you normally wear can stay on.
- Make sure only your face is in view.

**During the capture**

- Hold the phone about 30 to 40 cm away, with your whole face in the picture.
- Look at the screen and hold still for a moment.
- If prompted, turn your head slowly and only a little.
- Keep the screen open until it says you are finished.

## Glasses and other changes

- **Everyday glasses:** enrol as the user normally appears. If they sometimes wear glasses and sometimes do not, capture part of the enrolment each way.
- **Sunglasses, hats, caps, hoods:** remove them for enrolment, and ideally for sign-in.
- **Face masks over the nose and mouth:** remove them for enrolment and sign-in. Face recognition is not designed to work with the lower face covered.

## Why an enrolment fails or comes out weak

| Cause | What the user notices | Fix |
|---|---|---|
| Strong light behind the user | The capture does not progress, or recognition is unreliable later | Turn round to face the light |
| Very dim room | The capture is slow or stalls | Add light |
| Face too far or too close | "Move closer" or "Move back" | Follow the hint |
| Two faces in view | "One face at a time" | Make sure only the user is visible |
| Moving a lot, or leaving the frame | The capture stalls or restarts | Hold still in front of the camera |
| Single quick capture in one light | Enrolment works, but recognition fails later in other light | Use a guided capture, or re-enrol in typical conditions |
| Screen closed early | Fewer samples than intended | Keep the screen open until the capture finishes |

After about a minute without progress, show tips: face the light, adjust the distance, one face only.

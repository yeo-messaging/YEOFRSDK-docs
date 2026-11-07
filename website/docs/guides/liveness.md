# Liveness

The liveness check judges whether the camera sees a real, present person rather than a photo, a screen or a mask. It is **passive**: the user is never asked to blink, smile or turn. They just look at the camera.

## What to expect

- **It needs a moment to start.** When a face appears, the first answer usually takes about a second, and occasionally a few seconds. Show "Checking…", not an error.
- **It judges a sequence of frames.** The check is continuous and does not rely on one snapshot. A brief dip does not flip the answer, because a confirmed answer is held for about a second.
- **It ends when the face goes.** The answer expires within about 1.5 seconds. A face that returns starts the check again.
- **It judges one face.** While a second face is in view, the frame is not accepted as live. The check resumes on its own when one face remains.

## What can make a real person read as not live

| Situation | What happens | What to tell the user |
|---|---|---|
| The first moment after the face appears | Not live during the warm-up | Nothing. Show "Checking…". |
| Strong light behind the head | The answer can flicker | Turn to face the light |
| Very dim light | Slower or unreliable answers | Add light |
| Too close, too far, or cut off | Less reliable, or not judged until the face is well placed | Follow the framing hint |
| Two faces in view | Not live until one face remains | One face at a time |
| Moving in and out of the frame | Each return restarts the warm-up | Stay in front of the camera |
| Fast movement, blur, or glare on glasses | Occasional short dips | Hold still for a moment |

## How your app should use it

1. **Use the SDK's answer.** Read `isLive`, or `status` where the plugin provides it. In native Android, use `faceState == Detected && isConfident()`.
2. **Never compare `confidence` with a number.** It is for display and debugging only, and its scale can change between SDK versions. Older versions used a bar of 0.71, which no longer applies.
3. **Combine it with recognition.** See [Recognise a user](recognise.md).
4. **Use the framing hint to guide, not to decide.** It says "move closer", not "fail".

## Use the live camera

The check is designed for a live camera stream.

- **Frames your app supplies** cannot prove the person is present. A recording fed in through your own pipeline defeats any liveness check.
- **Still-image matching** (`matchImage`, `enrollImage`, `imageMatchScore`) runs no liveness at all.

## What it does and does not promise

The check is designed to refuse common presentation attacks, such as printed photos and photos or videos shown on phone, tablet and laptop screens. Like every liveness system, it lowers the risk and does not remove it.

It looks at what is in front of the camera. It does not check where the frames come from, so it cannot tell a real camera from a virtual one. See [Device and app security](../data-and-privacy.md#device-and-app-security).

For high-value actions, use it together with your app's other security measures.

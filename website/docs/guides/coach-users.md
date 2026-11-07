# Coach your users

The SDK reports what it sees, and your app owns the wording. These messages are a good starting point. Most failed attempts come from lighting and position, and a clear message fixes many of them.

## On-screen messages

| What the SDK reports | What to show |
|---|---|
| No face | "Look at the camera" |
| `tooFar` | "Move closer" |
| `tooClose` | "Move back" |
| `multipleFaces` | "One face at a time" |
| Badly placed, with no specific hint | "Adjust your position" |
| A face in view, liveness not confirmed yet | "Checking…" |
| Live, but not the enrolled person | "Not recognised" |
| `trusted`, or live with the expected name | Your success state |

!!! tip "Do not say 'spoof detected'"
    Real users see a "not live" reading during the warm-up. Show "Checking…" for the first few seconds, and show a failure only after the attempt time runs out.

## Give the user time

- Allow **5 to 10 seconds** per attempt.
- During the attempt, show the framing hint and "Checking…".
- If time runs out, show the tips below and offer a retry.
- After a few failed attempts, offer your fallback sign-in.

## If the right person is not recognised

Tell the user:

1. Face a light, not a window. Remove sunglasses, a hat or a mask.
2. Hold the phone at eye level, about 30 to 40 cm away, and follow the on-screen hint.
3. Keep still and look at the screen for a second or two.
4. Make sure no one else is in view.
5. If it keeps failing in your usual place, set up face sign-in again there.

## What affects recognition

| Factor | Effect | Advice |
|---|---|---|
| Light different from enrolment | The most common cause of a missed match | Face the light, and enrol in more than one light |
| Head angle | Looking well away from the camera lowers the match | Look at the screen, and enrol with small head turns |
| Distance | Small faces carry less detail, and cut-off faces cannot be compared | Follow the framing hint |
| Glasses, hats, masks | Can lower the match a lot | See [Enrol a user](enrol.md#glasses-and-other-changes) |
| Ageing, a new beard, new glasses | Gradual change may be absorbed, but a sudden large change may not | Re-enrol after a large change |
| Dirty lens | Lowers the match | Clean the lens |

## A support reply you can reuse

> After the update, face sign-in needs to be set up once more. Open the app, choose *Set up face sign-in*, face a light, and follow the prompts.

Use this wording when you [move users to a new version](upgrade.md).

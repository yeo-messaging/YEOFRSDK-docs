# Verify throughout a session

A login proves who opened a session. It does not prove who is still there. Because the SDK reports on every analysed camera frame, your app can keep checking that the right person is present, without asking the user to do anything.

## One-time and continuous checks

| | One-time check | Continuous check |
|---|---|---|
| **When it checks** | Once, at login or at one action | Throughout the period you choose |
| **What it shows** | The right person opened the session | The right person is still present |
| **If the phone changes hands** | Not noticed | Your app can react |
| **What the user does** | Takes a deliberate action | Nothing, in normal use |

It adds to your existing login and identity checks. It does not replace them.

## Choose when it runs

You set the policy. Common choices:

- **The whole session,** from sign-in to sign-out.
- **Sensitive screens,** such as account details or documents.
- **Critical moments,** such as a high-value payment or a change to account settings.

**Our advice:** start with critical moments, then widen the policy once you have seen how it behaves with your users. The camera must be running throughout, so a narrower policy is easier on the battery and easier to explain.

## Decide what happens when the person is gone

The SDK gives your app the signal. Your app decides the response, for example:

- ask the user to verify again;
- hide sensitive content until the right person returns;
- block high-risk actions;
- flag the session for review;
- end the session.

Use a stronger response for riskier actions. A person glancing away should not lose their session.

## How to read the signal

Use the same rule as at sign-in: **live, and the expected person.** See [Recognise a user](recognise.md) for the rule on each platform.

| What your app sees | What it probably means |
|---|---|
| Live, with the expected name | The right person is present |
| No face | The person looked away or left |
| Not live, or framing is off | The person moved, or the check is restarting |
| Live, with another name or no name | Someone else may be there |
| More than one face | Someone else has come into view |

### Allow a grace period

Real users look away, move and pass through poor light, so do not react to one frame. When a face leaves, the match drops within a fraction of a second and the liveness answer expires within about 1.5 seconds. A face that returns needs about a second to be confirmed again.

Start with a grace period of a few seconds, and tune it by testing with your own users:

```ts
let lastConfirmed = Date.now();

function onResult(r) {
  if (r.status === 'trusted' && r.matchedName === expectedUser) {
    lastConfirmed = Date.now();
  } else if (Date.now() - lastConfirmed > GRACE_PERIOD_MS) {
    onPersonNotPresent();   // your policy
  }
}
```

This example uses the React Native, Capacitor and Cordova result fields. On other platforms, use [the rule for your platform](recognise.md#the-rule-on-each-platform).

## What to plan for

- **The app must be open.** Expect no results while your app is in the background, and treat the return to the foreground as a new check.
- **Tell your users.** Explain that the camera stays on during the session and why. Cover it in your privacy notice.
- **Battery and heat.** Camera use and face processing draw power. Test sessions as long as your real ones, on the oldest phones you support.
- **Lighting changes.** A long session can cross poor light, so make sure your response to a failed check is not harsh. See [Coach your users](coach-users.md).
- **Capacitor and Cordova.** The native camera preview sits behind your page, there is one camera session per app, and the screen stays in portrait. Plan your screens around this.

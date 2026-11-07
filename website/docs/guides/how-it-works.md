# How it works

A face sign-in has four parts. All four run on the phone.

1. **Enrolment.** The user looks at the camera once. The SDK turns the face into numeric samples.
2. **Saving.** Your app saves the samples, called the gallery, and restores them at each launch.
3. **Recognition.** At sign-in, the SDK compares the face in view with the gallery.
4. **Liveness.** At the same time, the SDK checks that a real person is in front of the camera, not a photo or a screen.

Your app accepts the user only when recognition and liveness agree.

## Words used in these docs

| Term | What it means |
|---|---|
| **Enrolment** | Capturing a user's face once so that it can be recognised later. |
| **Sample** | A compact numeric description of a face, computed on the phone from one camera frame. It is not a photo. |
| **Gallery** | The set of enrolled people and their samples. It is what your app saves and restores. The SDK's calls name it the *tracker*, as in `saveTracker` or `loadTracker`. |
| **Recognition** | Comparing the face in view with the gallery, and naming the person when the similarity is high enough. |
| **Liveness** | Judging whether the camera sees a real, present person rather than a photo, a screen or a mask. |
| **Framing hint** | Advice on where the face is: too far, too close, more than one face, or no face. It helps you guide the user. |

## Who does what

| | The SDK | Your app |
|---|---|---|
| **Camera** | Runs it on iOS `FaceTrustService`, React Native, Capacitor and Cordova | Runs it on native Android, Flutter and any custom frame input |
| **Decision** | Reports recognition and liveness for each frame | Decides what to accept, and how long it must hold |
| **Screens** | Reports progress and hints | Owns all wording, prompts and layout |
| **Gallery** | Produces it and reads it back | Decides where it is kept and encrypts it |
| **Identity** | Compares faces | Decides who the user is before enrolment |
| **Device trust** | Does not check it | Checks for rooted or tampered devices if you need that |

## How recognition decides

Each face becomes a sample, and the SDK compares it with every sample stored for each enrolled person. The best similarity counts, and higher means more alike. A match needs a similarity of at least **0.35**, the default set by YEO from internal testing.

You do not need to pick a threshold. Use the result the SDK gives you. See [Recognise a user](recognise.md).

## Getting better over time

Each person can hold up to 25 enrolment samples and up to 25 recent samples. Enrolment samples never change. Recent samples can be refreshed by successful recognitions, at most one per second, and the oldest is replaced first. Recognition can then follow gradual changes in lighting or appearance while staying anchored to the enrolment.

| Platform | How recent samples behave |
|---|---|
| Android, on every plugin | Learned while recognising. Kept only if your app saves the gallery after the session. |
| iOS camera (`FaceTrustService`, React Native, Capacitor, Cordova) | Learned while the app runs. The built-in service saves its file only at enrolment and deletion, so they are lost at the next launch unless your app saves the gallery itself. |
| Flutter on iOS, and any custom frame input on iOS | Not learned. |

**Our advice:** plan for a good enrolment, and treat learning as a bonus.

## Different on Android and iPhone

| | Android | iPhone |
|---|---|---|
| **Face detection** | Google ML Kit, through Google Play services | Apple Vision, part of iOS |
| **Depth sensor** | Not used | Part of the decision on the iOS camera path, when the phone has TrueDepth. Not used in Flutter. |
| **Too close or too far** | A hint only in native code. Bridges report `badFraming`. | The built-in camera path reports `badFraming` and does not judge the face until it is well placed. |

These differences are why we recommend testing on both platforms, with your own screens.

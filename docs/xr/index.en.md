# XR

XR is not "a screen on your face". The usual assumptions stop working: there is no frame
to compose inside, the viewer's head is the camera, and the body is part of the input.
This section covers the numbers and the vocabulary that make the difference legible.

## Three numbers that shape every XR decision

| Number | Rough value | What it decides |
| --- | --- | --- |
| **IPD** — interpupillary distance | 58–72 mm per person | Stereo separation; a wrong setting makes the world feel the wrong size and tires the eyes |
| **Latency** — motion to photon | target under ~20 ms | Whether the world feels *solid* or *swimmy*. Nothing else in the project matters if this is bad |
| **FOV** — field of view | ~100° per eye on a Pico 4 | How much you have to fill, and how much peripheral vision can be used for comfort |

Scale is the consequence of the first number: the same virtual object at the same distance
reads as bigger or smaller depending on the headset's stereo separation. This is why VR
prototyping starts with a grey-box room at human scale, not with art.

## Comfort is a design constraint, not a setting

The body objects when what it sees and what its inner ear feels disagree. Three patterns
that follow:

- **Smooth locomotion** moves the world past a stationary body — *vection*. Many people are
  fine; some are not, and there is no way to tell from the outside.
- **Snap turn** trades smoothness for comfort: rotate in discrete jumps so the visual change
  is brief rather than sustained. Standard, and usually the first thing to add.
- **Acceleration and vertical motion** are the worst offenders. Constant velocity is
  tolerable; being pushed, dropped or lifted is not.

Camera shake, head-bob and forced camera movement are normal in flat games and usually wrong
in VR.

## Two interaction metaphors

- **Ray** — aim from the controller or the gaze at something far away. Good for menus,
  selection, distance. Cheap to implement, easy to learn.
- **Grab / direct** — reach out and take something near you. Good for manipulation, feels
  physical, needs correct hand poses and colliders.

Almost every interaction in the labs is one of these two, plus feedback: a highlight, a
haptic pulse, a sound. Feedback is what makes a grab feel like a grab.

## Vocabulary

XR is where English terminology hurts most, because many terms have no everyday Chinese
equivalent and are learned as vocabulary: *presence*, *embodiment*, *vection*. The
[glossary](../glossary/index.md) has an XR section with one-line meanings — start there
rather than with a paper.

## Planned pages

| Page | Status | Why it is here |
| --- | --- | --- |
| Scale and the grey-box room | planned | Why a room must be built at real size before anything else |
| Quaternions without the maths degree | planned | Why rotations are not three angles, and what to do with them anyway |
| Comfort patterns, compared | planned | Snap turn, teleport, vignette, and what each one trades away |
| Spatial audio in a room | planned | Occlusion, distance, and why the audio lab is not decoration |
| A frame budget for a Pico 4 | planned | What has to be cut first when the headset cannot keep up |

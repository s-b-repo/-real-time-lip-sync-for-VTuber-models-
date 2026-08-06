# -real-time-lip-sync-for-VTuber-models-

A tiny JavaScript sketch (see `basic code`) toward a browser-based
lip-sync driver for a VTuber avatar: receive text over a WebSocket,
speak it with the Web Speech API, and drive a mouth animation from an
audio-analysis loop.

> **Status: sketch, not a working library.** As written, `basic code`
> throws at runtime and produces no visible avatar. If you're looking
> for a working solution, jump to [Real alternatives](#real-alternatives)
> below.

## What `basic code` does — and doesn't

Correct pieces:
- Sets up an `AudioContext` and connects a gain node to the destination.
- Opens a WebSocket to `ws://localhost:8080` and speaks the received
  text via `SpeechSynthesisUtterance`.
- Grabs the microphone via `getUserMedia`, wires an `AnalyserNode`,
  and calls a mouth-animation function on `requestAnimationFrame`.

Broken pieces:
- **Line 25** — `audioContext.createMediaStreamSource(utterance)` is
  a type error. `createMediaStreamSource` takes a `MediaStream`;
  `SpeechSynthesisUtterance` is not one. `speechSynthesis.speak(...)`
  cannot be captured by Web Audio at all in the current browser APIs.
- **Lines 30-34** — `animateMouth(level)` is a stub. There's no
  rendered avatar, no SVG, no `<canvas>`, no VRM/Live2D binding — the
  level goes nowhere.
- **No HTML file** to load the script from.
- **No fallback** if the WebSocket connect fails; the whole flow
  silently stops.

## Real alternatives

If you want a working real-time VTuber lip-sync in a browser:

- **[three-vrm](https://github.com/pixiv/three-vrm)** — Pixiv's VRM
  loader on top of three.js. Drive `expressionManager.setValue('aa',
  level)` from the same audio-analyser you already have and the model
  opens its mouth.
- **[Inochi2D](https://inochi2d.com/)** — Live2D-alternative with a
  free web renderer.
- **[VSeeFace](https://www.vseeface.icu/)** — desktop app that already
  handles VRM + lip-sync from mic input; scriptable via OSC. Use this
  if you don't need to stay in a browser.
- **[MediaPipe FaceLandmarker](https://developers.google.com/mediapipe/solutions/vision/face_landmarker)** —
  when you want the *user's* face to drive the avatar's expressions,
  not just their mic level.
- Reading list: [wawa/talking-head-three-vrm](https://github.com/wawa-samples/talking-head-three-vrm)
  demo — closest thing to what `basic code` was aiming for, in ~150
  lines that actually run.

## Files

- `basic code` — the sketch. Preserved unchanged. Rename mentally as
  `.js` before opening.

## License

MIT — see [LICENSE](LICENSE).

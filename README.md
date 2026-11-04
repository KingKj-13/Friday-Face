# Friday Face

A holographic 3D head for the **F.R.I.D.A.Y.** voice assistant, running in the browser with Three.js.

- Hologram look: glowing edge, scanlines, sparkle dots, a faint wireframe, flicker and an occasional glitch
- Glowing lips with lip-sync (15 mouth shapes, such as OH, EE, M/B/P and F/V)
- A glowing brain inside the skull: about 150 neurons with signals travelling between them, getting busier while Friday talks
- Blinks and moves its head on its own; drag to rotate
- A **Glow** slider for brightness

## Run it

The page loads `face.glb`, so it has to be served over http (opening `index.html` directly as a file won't work):

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000.

To host it free, turn on **GitHub Pages** for this repo (Settings → Pages → Deploy from branch → `main`, root folder).

## Using it

- **Speak**: the browser reads your text aloud, and the mouth shapes are worked out from the letters and timed to each spoken word. Timing is best in Chrome and Edge.
- **Play audio**: pick any voice recording; the mouth shape is chosen live from the sound's frequencies and loudness.

## How it works

| Part | Where in `index.html` |
| --- | --- |
| Hologram shader (glowing edge, scanlines, glitch) | `holoMat` |
| Lip glow (vertices that move most when the mouth puckers) | `markLips()` |
| Even sparkle dots over the skin, moving with the face | `surfaceDust()` |
| Mouth shapes mapped onto the face's 52 expression shapes | `VISEMES` |
| Text to mouth shapes | `wordToVisemes()`, `scheduleText()` |
| Audio to mouth shapes | `audioViseme()` |
| Brain point cloud, neurons and travelling signals | `buildBrain()` |

Three.js 0.160.0 loads from the jsDelivr CDN; there is no build step.

## Ideas for next steps

- Exact lip-sync from a voice service that reports mouth shapes, such as Azure Speech
- Connect a real AI voice pipeline (speech-to-text → Claude → text-to-speech)
- Swap in a custom face made in Blender or VRoid with the same 52 ARKit expression shapes

## Credits

- Face model: "facecap" by **Bannaflak**, licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), from the [three.js examples](https://github.com/mrdoob/three.js/tree/dev/examples/models/gltf). In `face.glb` its textures and compression were removed; the mesh and expression shapes are unchanged.
- [Three.js](https://threejs.org/) (MIT)

## Agent Office Hub

`office/index.html` is a separate page: an isometric 3D office where bean-shaped AI agents work at cubicle desks (with headphones on) or play pool, table tennis and basketball, work out, drink coffee and rest. Click an agent to see its tasks and log, or edit its name, role, type (worker or relax only) and favourite activity.

With GitHub Pages on, it is at `https://kingkj-13.github.io/friday-face/office/`. It needs no build step and loads Three.js from a CDN.

Your own code can drive it from the page with `window.agentOffice` (`create`, `update`, `assignRole`, `call`, `log`, `complete`, `remove`, `list`), or from a parent page that embeds it in an iframe with `postMessage({agentOffice: "create", args: [{...}]})`.

# One Piece — Devil Fruit Hand Tracker

A real-time hand-tracking particle experience built with MediaPipe Hands, Three.js, and Web Audio API. Show your hand a gesture and awaken a Devil Fruit power — each one with a unique particle physics system, synthesized sound, and bloom post-processing.

No build step. No dependencies to install. Single HTML file, runs in any modern browser.

---

## Demo

Open `index.html` in a browser, allow camera access, and show your hand to the webcam.

---

## Devil Fruits

| Key | Gesture | Fruit | User | Translation |
|-----|---------|-------|------|-------------|
| `1` | ✌️ Two fingers up | Mera Mera no Mi | Portgas D. Ace | Flame-Flame Fruit |
| `2` | 🧊 Karate chop (palm sideways) | Hie Hie no Mi | Kuzan — Aokiji | Chilly-Chilly Fruit |
| `3` | 🤜 Fist | Gura Gura no Mi | Edward Newgate — Whitebeard | Tremor-Tremor Fruit |
| `4` | 🤚 Open palm facing camera | Ope Ope no Mi | Trafalgar D. Water Law | Op-Op Fruit |
| `5` | 🤏 Claw (fingers curled) | Yami Yami no Mi | Marshall D. Teach — Blackbeard | Dark-Dark Fruit |
| `6` | ☝️ One finger pointing | Pika Pika no Mi | Borsalino — Kizaru | Glint-Glint Fruit |
| `7` | 👇 Open palm facing down | Suna Suna no Mi | Crocodile | Sand-Sand Fruit |
| `8` | 🤲 Two hands stretched apart | Gomu Gomu no Mi | Monkey D. Luffy | Gum-Gum Fruit |
| `0` | — | Neutral (Grand Line sea) | — | — |

You can also press the number keys to preview any fruit without needing a camera.

---

## How Each Fruit Works

**Mera Mera no Mi** — 6 spiraling flame arms shoot upward from a molten white-orange core. Ember sparks scatter wide. The fire-ring accent halo spins and flickers every frame. Crackling noise burst on activation.

**Hie Hie no Mi** — 7 hexagonal ice crystal pillars rise from a frozen ground plane. Flying angular shards orbit the formation. Rotation is slow and stiff with random micro-snaps (ice cracking). Crystal ping sound on activation.

**Gura Gura no Mi** — A dense golden crackling sphere with rocky debris orbiting at mid-range. A shockwave ring expands outward from the center (~90 frames), physically jolting nearby particles as it passes, then resets and fires again. Deep 55 Hz rumble with distortion on activation. No rotation — it would break the radial illusion.

**Ope Ope no Mi** — A glowing ROOM sphere shell (brighter at the equator) with a 4-armed tetrahedron orbiting inside. A full equatorial ring accent layer rotates independently. Surgical sparkles fill the interior. Sweeping sine glide on activation.

**Yami Yami no Mi** — Particles spread across a wide sphere and are pulled inward each frame with real 1/r² gravity. Particles that collapse to the core respawn at the far edge, creating a continuous in-fall. The vortex accent ring spirals tighter over time. Reverse-envelope low drone on activation.

**Pika Pika no Mi** — 16 radial light beams fire outward from a blazing core, with 32 thinner secondary beams filling the gaps. Particles **teleport** on switch (no lerp transition). Beam brightness pulses at 18 Hz. Instant 3800 Hz beep on activation.

**Suna Suna no Mi** — A dune ground plane with two overlapping sine waves, 5 swirling sand columns that sway over time, and a dense field of airborne grain scatter. Gold dust sparkles sit on the dune crests. Lowpass-swept white noise on activation.

**Gomu Gomu no Mi** — A 3D rubber lattice grid (~29³ layout). When two hands are detected, the lattice warps along the axis between your wrists in real time — stretch your hands apart to pull the mesh. Springy pitch-drop boing on activation.

---

## Technical Stack

| Layer | Library |
|-------|---------|
| 3D rendering | [Three.js r160](https://threejs.org) via importmap |
| Post-processing | `UnrealBloomPass` via Three.js addons |
| Hand tracking | [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands) |
| Audio | Web Audio API (synthesized, no external files) |

**Particle counts:** 25,000 primary + 3,000 accent per fruit  
**Physics:** Per-frame velocity integration for Yami (gravity), Gura (shockwave), Gomu (spring stretch), plus live animation ticks for Mera (flicker), Suna (sway), Pika (pulse), Hie (micro-snaps)

---

## Gesture Classifier

Gestures are evaluated in priority order to avoid conflicts:

1. Gomu Gomu — two hands detected, both open, wrists far apart
2. Gura Gura — all fingers curled (fist)
3. Yami Yami — fingers partially curled (claw)
4. Pika Pika — only index finger extended
5. Mera Mera — index + middle extended
6. Hie Hie — all fingers extended, palm sideways (not facing camera)
7. Ope Ope — all fingers extended, open palm facing camera
8. Suna Suna — all fingers extended, palm facing down
9. Neutral — no hands or no match

A 4-frame hold debounce prevents flickering between gestures.

---

## Running Locally

```bash
# Any static file server works — e.g.:
npx serve .
# or
python -m http.server
```

Then open `http://localhost:3000` (or whichever port). Camera permission is required for hand tracking. All assets load from CDN — internet connection needed on first load.

> **Note:** Opening `index.html` directly as a `file://` URL will block camera access in most browsers due to security restrictions. Use a local server.

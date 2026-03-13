# 🎵 Anima

> *Music that listens back.*

Anima reads your emotional state — through your webcam, wearable, or a mood check-in — and generates original ambient music in real time that either mirrors how you feel or gently guides you somewhere better.

No playlists. No loops. Always original.

---

## What Makes It Different

Most music apps *recommend*. Anima *creates*. Every session produces music that has never existed before, tuned specifically to your emotional arc. As your mood shifts, the music shifts with it — same key, same session, but morphing continuously rather than cutting to something new.

There are two modes:

- **Mirror** — the music reflects you. Anxious gets dissonant pads and irregular rhythm. Calm gets warm, slow-attack sine tones. The music becomes a sonic portrait of your inner state.
- **Shift** — the music moves you. You set a destination mood, and Anima composes a gradual sonic bridge to get you there over minutes, not seconds.

---

## How It Works

### 1. Emotion Detection

Anima reads your emotional state from three optional inputs:

| Source | What It Detects | Privacy |
|--------|----------------|---------|
| Webcam | Facial expression → valence, arousal | 100% local, no video transmitted |
| Wearable | Heart rate + HRV → stress/recovery | Read via Web Bluetooth |
| Manual | Emoji picker / mood slider | Always available as fallback |

Webcam processing runs entirely in-browser using **MediaPipe Face Mesh**. Your video never leaves your device.

Emotion is sampled every 5 seconds and mapped to a continuous 2D space (valence × arousal). A confidence threshold prevents the music from reacting to fleeting micro-expressions.

### 2. Music Generation

The music engine is built on **Tone.js** for real-time synthesis and scheduling, with **Magenta.js (MusicRNN)** generating melodic sequences. Your emotional state drives a set of musical parameters:

```
Valence   →  Key (minor ↔ major), brightness, harmonic tension
Arousal   →  Tempo, note density, attack/release envelope
Stress    →  Reverb depth, dissonance, rhythmic irregularity
```

When your mood transitions, parameters morph over 30–60 seconds rather than cutting. This is what makes the music feel alive and intentional rather than algorithmic.

### 3. Session Timeline

At the end of every session, Anima shows you a visual timeline: your mood arc overlaid against the music it generated. You can replay any moment. You can save it. No two sessions are ever the same.

---

## Tech Stack

```
Frontend     React + TypeScript
Audio        Tone.js + Magenta.js (MusicRNN)
Vision       MediaPipe Face Mesh (WASM, runs in-browser)
Wearables    Web Bluetooth API
Backend      Node / Express  (auth, session storage only)
Storage      PostgreSQL (session metadata) + S3 (audio exports)
```

All audio generation and emotion detection runs client-side. The server handles authentication and optional session persistence — nothing sensitive touches it.

---

## Getting Started

### Prerequisites

- Node.js 18+
- A modern browser with WebGL support (Chrome or Edge recommended for MediaPipe)
- Optional: A BLE heart rate monitor for wearable input

### Installation

```bash
git clone https://github.com/yourname/anima
cd anima
npm install
```

### Run Locally

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

On first launch, Anima will ask for camera permission. You can decline and use manual mood input instead — no features are locked behind camera access.

### Environment Variables

```bash
# .env.local
NEXT_PUBLIC_APP_URL=http://localhost:3000
DATABASE_URL=postgresql://...       # optional — for saving sessions
S3_BUCKET=...                       # optional — for exporting audio
```

---

## Project Structure

```
anima/
├── src/
│   ├── emotion/
│   │   ├── FaceDetector.ts        # MediaPipe pipeline
│   │   ├── WearableReader.ts      # Web Bluetooth + HRV
│   │   └── MoodFusion.ts          # Combines signals → mood vector
│   ├── music/
│   │   ├── MusicEngine.ts         # Tone.js synth + scheduler
│   │   ├── MoodProfiles.ts        # Mood → musical parameter mappings
│   │   ├── MelodyGenerator.ts     # Magenta MusicRNN wrapper
│   │   └── Morpher.ts             # Smooth parameter transitions
│   ├── session/
│   │   ├── Timeline.ts            # Records mood + music events
│   │   └── Exporter.ts            # Renders session to audio file
│   └── ui/
│       ├── MoodDisplay.tsx
│       ├── MusicVisualizer.tsx
│       └── SessionReplay.tsx
├── public/
│   └── models/                    # MediaPipe + Magenta model weights
└── server/
    └── routes/                    # Auth + session API
```

---

## Mood Profiles

Anima ships with six core profiles. You can extend them in `src/music/MoodProfiles.ts`.

| Mood | Key | Tempo | Timbre | Notes |
|------|-----|-------|--------|-------|
| Calm | Pentatonic major | 55–65 bpm | Warm sine, slow attack | High reverb, sparse |
| Focused | Dorian mode | 75–85 bpm | Clean piano, light pad | Minimal reverb |
| Happy | Major | 95–110 bpm | Bright pluck, marimba | Short reverb, dense |
| Sad | Natural minor | 50–60 bpm | Strings, detuned piano | Heavy reverb, slow |
| Anxious | Phrygian | 110–130 bpm | Dissonant pad, glitch | Irregular rhythm |
| Tired | Lydian | 45–55 bpm | Ambient wash, drones | Blurred, formless |

---

## Roadmap

- [ ] iOS / Android app (React Native + native BLE)
- [ ] Biometric support: Oura Ring, Apple Watch, Garmin
- [ ] Collaborative sessions — two people's moods shaping one piece
- [ ] Offline model — full generation with no CDN dependency
- [ ] Therapist dashboard — mood trend reports for clinical use

---

## Privacy

Anima is built privacy-first:

- **Webcam video never leaves your device.** Emotion processing runs locally in WASM.
- **No emotion data is stored** unless you explicitly save a session.
- **Sessions are end-to-end encrypted** if stored on our servers.
- **You can run entirely offline** — all models can be bundled locally.

---

## Contributing

Pull requests welcome. Please open an issue first for significant changes.

```bash
npm run test        # unit tests
npm run test:e2e    # end-to-end (requires browser)
npm run lint
```

---

## License

MIT © 2025 — built with Tone.js, Magenta, and MediaPipe.

---

*Anima: from Latin — soul, breath, the animating principle.*
*Music has always known something about you that you hadn't said yet.*

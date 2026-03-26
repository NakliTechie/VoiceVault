# VoiceVault

Transcribe audio in your browser — offline, private, no API keys needed.

🎙️ **Try it now →** https://naklitechie.github.io/VoiceVault/

VoiceVault is part of the **NakliTechie** series of browser-native tools: single-file apps that run entirely on your device. No server. No API keys. No data leaving your machine.

---

## Features

- **Multiple Input Methods** — Drag & drop audio files, record directly from your microphone, or paste a URL to remote audio.
- **Offline Transcription** — Uses OpenAI's Whisper model running entirely in your browser via Transformers.js. Once cached, works completely offline.
- **Multiple Export Formats** — Export transcriptions as TXT, SRT (subtitles), VTT (web captions), or JSON (with timestamps).
- **Audio Playback** — Play back your original recordings directly from the transcription history.
- **Retranscribe** — Change language or model settings and re-run transcription on any saved recording.
- **Transcription History** — All transcriptions are stored locally in your browser with datestamped organization.
- **Timestamped Segments** — Every word is timestamped — useful for subtitles, meeting notes, or interview transcripts.
- **Confidence Scores** — Optional highlighting of low-confidence words so you know which parts to double-check.
- **Batch Processing** — Queue multiple audio files and transcribe them sequentially.
- **Privacy First** — No audio ever leaves your device. Toggle auto-delete to avoid storing recordings.

---

## Supported Audio Formats

VoiceVault supports any audio format your browser can decode:

- **MP3** — Most common audio format
- **WAV** — Uncompressed audio
- **WebM** — Default recording format for microphone input
- **OGG** — Open source audio format
- **M4A** — AAC audio (Safari/iOS)
- **FLAC** — Lossless audio

---

## Models

| Model | Size | Speed | Accuracy | Best for |
|-------|------|-------|----------|----------|
| **whisper-tiny** | ~150 MB | Fast | Good | Quick transcriptions, shorter audio |
| **whisper-base** | ~250 MB | Balanced | Better | Default choice, most use cases |

Both models are quantized (8-bit) for faster browser inference. The model is downloaded from Hugging Face on first use and cached indefinitely.

---

## How It Works

VoiceVault uses **Transformers.js** to run OpenAI's Whisper speech recognition model entirely in your browser:

1. **First Load** — The Whisper model (~250 MB for whisper-base) is downloaded from Hugging Face CDN and cached in your browser's IndexedDB.
2. **Audio Processing** — Your audio is decoded and resampled to 16kHz (Whisper's native sample rate) using the Web Audio API.
3. **Transcription** — The model processes the audio in 30-second chunks with overlapping windows to handle long recordings.
4. **Storage** — Transcriptions and optional audio recordings are stored in IndexedDB for offline access.
5. **Export** — Transcriptions can be exported in multiple formats, all generated client-side.

No audio data is ever sent to any server — not even to Hugging Face. The model runs entirely via WebAssembly in your browser.

---

## Running Locally

No build step. No dependencies. Just open the file:

```bash
# Option 1: Open directly in browser
open voicevault.html

# Option 2: Serve with Python
python3 -m http.server 8000
# Then visit: http://localhost:8000/voicevault.html

# Option 3: Serve with Node.js (npx)
npx serve .
```

---

## Deployment

Host on any static file server:

- **GitHub Pages** — Push to a GitHub repo and enable Pages
- **Netlify** — Drag and drop the HTML file
- **Vercel** — Connect your repo
- **Local server** — `python3 -m http.server`

The app is a single HTML file with no build process.

---

## Browser Support

VoiceVault works in modern browsers that support:

- Web Audio API
- IndexedDB
- WebAssembly
- MediaRecorder API (for microphone recording)
- Transformers.js

**Tested on:**
- ✅ Chrome 120+
- ✅ Firefox 120+
- ✅ Safari 17+
- ✅ Edge 120+

---

## Privacy & Security

- **No server calls** — All processing happens locally
- **No analytics** — No tracking, no telemetry
- **No data collection** — Your audio and transcriptions stay on your device
- **Optional storage** — Toggle auto-delete to prevent audio from being stored
- **Clear anytime** — Delete individual transcriptions or clear all history

---

## Limitations

- **Audio length** — Very long recordings (>1 hour) may take significant time to transcribe
- **Speaker diarization** — Cannot distinguish between different speakers (not yet supported in browser Whisper)
- **Background noise** — Whisper performs best with clear speech; heavy background noise may reduce accuracy
- **First load time** — Initial model download (~250 MB) takes time depending on your connection
- **Memory usage** — Large audio files require significant RAM for processing

---

## Part of the NakliTechie series

| Tool | What it does |
|------|--------------|
| [**BabelLocal**](https://github.com/NakliTechie/BabelLocal) | Offline translation — 200 languages, NLLB model |
| [**StripLocal**](https://github.com/NakliTechie/StripLocal) | EXIF metadata stripper — nothing leaves the browser |
| [**GambitLocal**](https://github.com/NakliTechie/GambitLocal) | Chess vs Stockfish — correspondence mode via URL |
| **VoiceVault** | Audio transcription — Whisper, offline-first |
| [**KingMe**](https://github.com/NakliTechie/KingMe) | English draughts — custom minimax AI, zero deps |
| [**SnipLocal**](https://github.com/NakliTechie/SnipLocal) | Background remover — RMBG-1.4, passport mode |
| [**Clacker**](https://github.com/NakliTechie/Clacker) | Split-flap display — browser-native, offline |
| [**Strait Command**](https://github.com/NakliTechie/strait-command) | Mine-clearing game in strategic waterways |
| [**Chokepoint**](https://github.com/NakliTechie/chokepoint) | Maritime tower defense — hold the strait |
| [**PredictionMarket**](https://github.com/NakliTechie/PredictionMarket) | Prediction market simulator — Parimutuel & LMSR |
| **PDFLOcal** | PDF editor — merge, split, rotate, annotate |
| **RangeLocal** | Missile range simulator — interactive globe & map |
| [**3D Tic-Tac-Toe**](https://github.com/NakliTechie/3d-tic-tac-toe) | 3D tic-tac-toe — 3×3×3 to 5×5×5, minimax AI |

---

## Tech Stack

| Concern | Solution |
|---------|----------|
| Speech recognition | Transformers.js + Whisper (Xenova models) |
| Model hosting | Hugging Face CDN (cached in browser after first load) |
| Audio processing | Web Audio API (decode, resample to 16kHz) |
| Storage | IndexedDB (transcriptions + optional audio) |
| Export formats | Native Blob + URL APIs |
| Hosting | Any static file server (GitHub Pages, Netlify, etc.) |
| Dependencies | Zero npm packages — pure vanilla JS |

---

## Why Browser-Native?

> *"Why are you uploading that to a server?"*

Meeting transcriptions, medical dictation, voice notes, interview recordings — these are exactly the audio files you don't want processed by a cloud API. OpenAI's Whisper API charges per minute and logs every request.

VoiceVault is the answer: **world-class transcription with nothing leaving your device.**

---

**Built by Chirag Patnaik**

[View on GitHub](https://github.com/NakliTechie/VoiceVault)

---

## About

Browser-native audio transcription using OpenAI's Whisper model via Transformers.js. No server-side components — everything runs locally in your browser.

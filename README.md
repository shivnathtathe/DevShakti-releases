<p align="center">
  <img src="assets/devshakti-banner.png" alt="DevShakti Banner" width="100%" />
</p>

<h1 align="center">DevShakti</h1>

<p align="center">
  <strong>Your private, offline AI assistant that runs entirely on your device.</strong>
</p>

<p align="center">
  <a href="#download"><img src="https://img.shields.io/badge/Platform-Android-3DDC84?logo=android&logoColor=white" alt="Android" /></a>
  <a href="#download"><img src="https://img.shields.io/badge/Version-1.0-blue" alt="Version" /></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-green" alt="License" /></a>
  <a href="#supported-models"><img src="https://img.shields.io/badge/Engine-llama.cpp-orange" alt="llama.cpp" /></a>
</p>

<p align="center">
  No cloud. No subscriptions. No data leaves your phone.
</p>

---

## What is DevShakti?

DevShakti is a fully offline AI assistant for Android. It runs large language models (LLMs) directly on your device using [llama.cpp](https://github.com/ggerganov/llama.cpp) — no internet connection required for inference. Your conversations, documents, and data never leave your phone.

Beyond chat, DevShakti can **control your device**: play music on Spotify, launch apps, toggle the flashlight, compose messages, search the web, perform calculations, check the weather, and query your own documents — all through natural language.

---

## Key Features

### On-Device LLM Inference
- Runs GGUF-quantized models locally via **llama.rn** (llama.cpp binding for React Native)
- Built-in **Model Manager** — browse, download, and switch between models
- Supports models from **1B to 8B+ parameters** with Q4_K_M quantization
- Zero cloud dependency for AI responses

### Retrieval-Augmented Generation (RAG)
- Upload PDFs, text files, and documents directly into the app
- Pure TypeScript **TF-IDF vectorizer** with cosine similarity search
- Ask questions about your documents — answers grounded in your actual content
- All document processing happens on-device

### Device Control via Natural Language
DevShakti understands commands like _"play lofi beats on Spotify"_, _"turn on the flashlight"_, or _"navigate to the nearest coffee shop"_.

| Tool | What It Does |
|------|-------------|
| **Calculator** | Mathematical calculations with safe expression parsing |
| **Weather** | Real-time weather via Open-Meteo API (free, no key needed) |
| **Time** | Current time for any timezone or location |
| **Web Search** | Internet search via DuckDuckGo + Wikipedia |
| **Document Search** | RAG-powered search across your uploaded documents |
| **Flashlight** | Toggle device torch on/off |
| **Media Control** | Play, pause, skip, next, previous, stop — works with any media app |
| **App Action** | Deep-link into apps — _"search X on YouTube"_, _"play X on Spotify"_ |
| **App Launcher** | Open any installed app by name |
| **Phone Dialer** | Open dialer with a number pre-filled |
| **SMS Composer** | Open messaging app with recipient + body pre-filled |
| **Email Composer** | Open email client with recipient, subject, body pre-filled |
| **Maps** | Search locations or get navigation directions |
| **Camera** | Open device camera |
| **Settings** | Jump to specific settings pages (WiFi, Bluetooth, Display, etc.) |

### Smart App Integration
- **Attach apps** to DevShakti via the app picker
- Built-in profiles for **YouTube, Spotify, YouTube Music, Apple Music, SoundCloud, Netflix, Prime Video, Google Maps**
- Per-app playback strategies using **Android MediaBrowser API** for background music control
- Extensible profile system — add any app with a deep link URI

### AI Personalities
Switch between 5 built-in roles that change how the AI responds:

| Role | Style |
|------|-------|
| **Assistant** | General-purpose, balanced helper |
| **Teacher** | Patient, educational, explains concepts |
| **Friend** | Casual, conversational companion |
| **Expert** | Technical, precise, detailed |
| **Coach** | Motivational, goal-oriented |

### Polished UI/UX
- **Dark and Light themes** with system-aware switching
- **Flash-list powered** chat for smooth scrolling on long conversations
- Text-to-speech responses via native TTS engine
- Markdown rendering in chat bubbles
- Typing indicators and message status

---

## Download

### Latest Release

> **[Download devshakti-v1.0.apk](https://github.com/ShivnathTathe/DevShakti/releases/latest)** from GitHub Releases

### Requirements

| | Minimum |
|---|---|
| **Android** | 7.0 (API 24) |
| **Architecture** | arm64-v8a |
| **RAM** | 2 GB (for 1B models), 4+ GB recommended |
| **Storage** | 100 MB for app + space for models (670 MB - 5 GB each) |

### Installation

1. Go to the [Releases](https://github.com/ShivnathTathe/DevShakti/releases/latest) page
2. Download the `.apk` file
3. Enable **Install from Unknown Sources** in your Android settings if prompted
4. Install and open DevShakti
5. Download a model from the built-in Model Manager
6. Start chatting

---

## Supported Models

All models use **Q4_K_M quantization** for the best balance of quality and speed on mobile hardware. Download directly within the app.

| Model | Parameters | Size | RAM Needed | Best For |
|-------|-----------|------|------------|----------|
| **TinyLlama 1.1B** | 1.1B | ~670 MB | ~1 GB | Quick responses, low-end devices |
| **Phi-2 2.7B** | 2.7B | ~1.6 GB | ~2.5 GB | Good balance of speed and reasoning |
| **Gemma 2B** | 2B | ~1.5 GB | ~2.2 GB | Strong instruction following |
| **Llama 3 8B** | 8B | ~4.9 GB | ~6 GB | Highest quality, flagship phones |

You can also manually place any GGUF model file on your device and load it through the app.

---

## Architecture

```
User Input
    |
    v
OrchestratorService         <- Fast-path regex + 4-step LLM orchestration
    |
    +---> Fast Path          <- Instant tool dispatch (no LLM call needed)
    |         |
    |         +---> ToolService ---> Calculator / Weather / MediaControl / ...
    |
    +---> LLM Path           <- Full inference via llama.rn
              |
              +---> Tool Detection ---> ToolService ---> Response
              |
              +---> Direct Response
```

### How It Works

1. **Fast-path detection** — Regex patterns instantly route commands like _"turn on flashlight"_ or _"pause music"_ to the right tool without LLM inference. Sub-100ms response times for device control.

2. **LLM orchestration** — For complex queries, the on-device LLM reasons about which tool to use, extracts parameters, and formats the response.

3. **RAG pipeline** — Documents are chunked, embedded using TF-IDF bag-of-words, and stored in a vector store. Queries are matched via cosine similarity and relevant chunks are injected into the LLM context.

4. **Media control** — Uses Android's `MediaBrowser` API to directly control music apps in the background. App-specific `PlayConfig` strategies ensure reliable playback across Spotify, YouTube Music, Apple Music, and others.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Framework** | React Native 0.80.1 (New Architecture, Hermes) |
| **Language** | TypeScript (strict mode, zero errors) |
| **LLM Engine** | llama.rn 0.6.1 (llama.cpp binding) |
| **State** | Zustand 5 + AsyncStorage |
| **UI List** | @shopify/flash-list |
| **Documents** | react-native-document-picker + react-native-fs |
| **TTS** | react-native-tts |
| **Embeddings** | TF-IDF bag-of-words (pure TypeScript) |
| **Vector Search** | Cosine similarity |
| **Weather API** | Open-Meteo (free, no API key) |
| **Web Search** | DuckDuckGo + Wikipedia APIs |
| **Media** | Android MediaBrowser API (native Kotlin module) |
| **Testing** | Jest (56 unit tests) |

---

## Permissions

DevShakti requests minimal permissions:

| Permission | Why |
|-----------|-----|
| `INTERNET` | Download models, weather data, web search (LLM runs offline) |
| `RECORD_AUDIO` | Voice input (future speech-to-text) |

No location, contacts, storage, or background service permissions.

---

## Privacy

- **All LLM inference runs on-device** — conversations never sent to any server
- **Documents stay local** — RAG processing happens entirely on your phone
- **No analytics, no telemetry, no tracking**
- **No account required** — install and use
- Internet is only used for: downloading models (Hugging Face), weather data (Open-Meteo), and web search (DuckDuckGo)

---

## Roadmap

- [ ] iOS support
- [ ] Speech-to-text input
- [ ] Multi-turn tool chains
- [ ] Custom tool builder
- [ ] Conversation export/import
- [ ] Home screen widget for quick commands

---

## Author

**Shivnath Tathe**

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---

<p align="center">
  Built with llama.cpp, React Native, and a belief that AI should be private by default.
</p>

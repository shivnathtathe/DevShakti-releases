# Changelog

All notable changes to DevShakti will be documented in this file.

## [1.0] - 2026-02-28

### Added
- On-device LLM inference via llama.rn (llama.cpp)
- Model Manager with 4 pre-configured GGUF models (TinyLlama 1.1B, Phi-2 2.7B, Gemma 2B, Llama 3 8B)
- Model download with progress tracking and cancellation
- RAG pipeline: document upload, TF-IDF embedding, cosine similarity search
- 15 built-in tools: Calculator, Weather, Time, Web Search, Document Search, Flashlight, Media Control, App Action, App Launcher, Phone Dialer, SMS Composer, Email Composer, Maps, Camera, Settings
- Smart app integration with MediaBrowser API for background music control
- App-specific PlayConfig strategies (Spotify, YouTube Music, Apple Music, SoundCloud)
- Deep link profiles for YouTube, Spotify, YouTube Music, Netflix, Prime Video, Google Maps, Apple Music, SoundCloud
- App attachment system with picker modal and chip UI
- 5 AI personality roles: Assistant, Teacher, Friend, Expert, Coach
- Fast-path regex routing for instant device control (sub-100ms)
- 4-step LLM orchestration pipeline
- Dark and Light theme support
- Text-to-speech responses
- Flash-list powered chat UI
- 56 unit tests passing
- TypeScript strict mode with zero errors

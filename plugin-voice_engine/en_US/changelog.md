---
layout: default
title: Voice Engine — Changelog
lang: en_US
pluginId: voice_engine
---

# Voice Engine changelog

## Version in development

### 28/11/2026

- **New features**: new **Wyoming** provider (Piper, Whisper… on your network, no cloud); much faster **Gemini** voice
  (first sound in under a second, whole sound in 1.5 s instead of 6 s); streamed sound for Mistral too; **Rephrase with
  AI** box (text rewritten for speech by ai_proxy); **Clone** button to create a voice from a recording (ElevenLabs,
  Cartesia, Mistral).
- **Improvement**: tables and bullet points are read aloud better.

### 27/11/2026

- **New features**: "Load voices" also works for 9router (Edge voices), ai_proxy (Marie voices) and OpenAI; choice
  of the box sound card; **sentence by sentence** playback on the box (a long text starts sooner); **streamed sound**
  to another plugin (Chromecast, Sonos…); new **speaker of another machine (SSH)** output; **listening with Gemini**.
- **Fixes**: cleaner end of short sounds on the box; clear message when a provider has run out of credit (instead of
  "key refused"); OpenAI listening moved to the model that replaces those shut down in February 2027.
- **New features and fixes**: "Import from AI Assistant" button in the configuration, which copies the provider keys
  (keys already entered are kept); the ai_proxy preset now finds the ai_proxy address and key on its own, and can now speak as well as listen
  (Mistral voices, with an up-to-date ai_proxy).
- **Fix**: the 9router preset now takes the key from the 9Router plugin on its own; without it, recent 9router
  versions refused to speak.

## 0.1.0 (2026-11-26)

- **First version**: one equipment per voice or listening profile, with the provider of your choice: 9router and
  ai_proxy (on the box), OpenAI, Groq, OpenRouter, ElevenLabs, Mistral Voxtral, Gemini, Cartesia or Google Translate (free).
- **Commands for your scenarios**: Say, Create file, Play sound, Transcribe.
- **Where the sound goes**: box speaker, or another plugin that plays a link (Chromecast, Sonos, voice satellite…).
  Links to sounds are protected and valid for one hour.
- **Lower costs**: the same sentence with the same voice is made (and billed) only once; daily character cap;
  fallback profile when a provider does not answer.
- **Voice of all Jeedom**: the plugin can replace the Jeedom TTS engine.
- **Test** (listen to a voice, try listening) and **Health** windows.
- Interface translated into English.

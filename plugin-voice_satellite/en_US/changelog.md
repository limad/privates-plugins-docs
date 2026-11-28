---
layout: default
title: Voice Satellite — Changelog
lang: en_US
pluginId: voice_satellite
---

# Voice Satellite changelog

## 0.4.0 (2026-11-25)

- **The satellite speaks while the AI is writing**: the first sentence is spoken as soon as it is complete, without waiting for the end of the reply. Measured on a virtual satellite: 3 s between the end of the question and the start of the reply, down from 5 s (five-sentence reply). Requires an up-to-date AI Assistant; otherwise the reply arrives in one piece, as before.
- Voice split into sentences: the end of a sentence no longer waits for the next one.
- Streamed reply also with Claude and Gemini as the provider of the AI Assistant MCP client, in addition to OpenAI-compatible providers.
- Plugin update: commands added by a new version (for example "Stop playback" and "Last error") are now created on satellites already configured, without having to save each one.
- Running timers are no longer lost when Jeedom sets a satellite's encryption key.

## 0.3.0 (2026-11-25)

- **Faster voice**: a long reply is spoken from its first sentence, the rest is synthesised while it plays. Short sentences already spoken are kept in memory.
- **Satellite room** (parent object) given to AI Assistant: "turn on the light" targets the satellite's room (requires an up-to-date AI Assistant).
- **Stop playback** and **Last error** commands.
- **Per-satellite** listening settings; listening and reply settings applied **without restarting** the daemon.
- **Timers kept** when the daemon or the box restarts.
- Faster equipment save, faster end-of-speech detection at wake word, more reliable encryption key setup.

## 0.2.0 (2026-11-25)

- **Search**: discovery of the ESPHome satellites on the network (mDNS), one-click add.
- **Encryption key** set from Jeedom (ESPHome 2026.7 or later), and the cause of connection failures is shown (key required, key refused, satellite not encrypted).
- **Volume / mute** commands, **wake word choice** (list read from the satellite).
- **Timers**: commands and voice ("set a timer for 10 minutes", in French), handled without the LLM, countdown and ring on the satellite.
- **Listens again** when the reply ends with a question.
- Satellites **Health** window.
- English translation.

## 0.1.0 (2026-11-24)

- First version (alpha): one equipment per ESPHome satellite, full voice session (silero end of speech, transcription, reply and voice through AI Assistant, conversion to the satellite format), Announce and Announce and listen commands, connection state.
